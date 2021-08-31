---
title: "Network your Pi"
teaching: 20
exercises: 10
questions:
- "How can you network your Pi?"
- "How can you access your Pi via the Internet?"
objectives:
- "Getting your pi on the Internet."
keypoints:
- "Use Ethernet and Wifi."
- "Use ssh and scp."
- "Tunnel connections via a DMZ."
---
We will configure your Pi to be accessible on the network and then connect to your pi securely to issue commands and exchange files.

## About Network Connections

Network connections allow computers to exchange data. For our purposes, its worth understanding three kinds of addresses. At the lowest level, each network interface has a unique Media Access Control (MAC) address. A modern Raspberry Pi comes with three network interface: a "loopback", an Ethernet interface and a Wifi interface. The loopback is used to exchange traffic between processes on the same computer. The others, when configured, will send and receive traffic via the network

When you connect an ethernet cable, or when your wifi controller associates with a Service Set Identifier (SSID) — or base-station — the computer will try to configure each network interface using the Dynamic Host Configuration Protocol (DHCP). It broadcasts on its network segment and the server with associate the MAC address with an IP address. It will generally also associate the IP address with a domain name.

You can see all of this information (and more) using the "ip" command. This command will show all of the interfaces.

~~~
$ ip addr
~~~
{: .language-bash}

The MAC address is a set of 8 pairs of hexidecimal digits, e.g. "link/ether 80:fa:5b:54:e2:41".

If the interface is configured, it will have an IP address, e.g. "inet 10.0.0.49". This is the address you will connect to via ssh.

## Ethernet

The fastest and most reliable want to network your Pi is via ethernet. Connect an ethernet cable with an RJ-45 connector between your Pi and an active ethernet jack. If DHCP service is available, your network connection will be automatically configured and you'll be on the network.

## Eduroam

Many campuses use eduroam to provide single-sign-on authenticated network connectivity that will work across multiple institutions. At some campuses, you may find that you are unable to ssh to a pi hosted on eduroam, however.

This recipe has been found to work at UMass, but might not work at other institutions that have eduroam configured differently. It assumes you have downloaded the eduroam certificate authority file and placed it on the "boot" volume of the pi (i.e. "eduroam.pem".)

~~~
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US
network={
    ssid="eduroam"
    proto=RSN
    key_mgmt=WPA-EAP
    pairwise=CCMP
    auth_alg=OPEN
    eap=TTLS
    anonymous_identity="xxxxxxxx@umass.edu"
    # adjust the following CA line as required to match your filename
    ca_cert="/boot/eduroam.pem"
    phase2="auth=PAP"
    identity="xxxxxxxx@umass.edu"
    password="xxxxxxxx"
}
~~~
{: .source}

## Using WPA/WPA2

Most home or public wifi networks use WPA/WPA2 for security, although the security is relatively limited. You can configure a raspberry pi to connect to these networks, if available by adding a clause to your wpa_supplicant.conf file:

~~~
network={
 ssid="[ssid]"
 psk="xxxxxxx"
}
~~~
{: .source}

## Tracking your Pi

If you're using your Raspberry Pi with a display, you can easily find your IP address. But if you're running your Pi "headless" (without a monitor or keyboard) you need another way to discover what IP address it has been assigned. This script can be installed (e.g. in /usr/local/bin)

~~~
#! /bin/bash
# rpi phone home script 20200228 sbrewer
for i in $(ls -1 /sys/class/net)
do
  #echo "Working on $i interface..."
  if [ $(cat /sys/class/net/$i/operstate) = "up" ]
  then
     echo "Reporting $i interface..."
     _IP=$(hostname -I)
     _HW=$(cat /sys/class/net/$i/address)
     _HN=$(hostname)
     /usr/bin/wget -O/dev/null -q https://bcrc.bio.umass.edu/pitrack/?hwaddr=$_HW\&pi=$_HN\&ip=$_IP
     fi
done
~~~
{: .language-bash}

## Making connections to private networks

Many networks (especially home networks) use private, "non-routable addresses." If your address starts with 10, 192, or 172, you probably have an address that can only be reached from other hosts on the same network. As long as you're connecting from the same network, everything will work fine. If you need to connect from a different network, there are several options.

A Virtual Private Network (VPN) will give a computer that resides on a different network a virtual address on the local network. This will allow you to connect to other hosts on the local network as if your computer were also on the local network.

Routers can be configured to advertise a port that will connect to a computer on the local network. In this way, you could connect to an arbitrary port on the public address of the router and your connection can be directed to port 22 (i.e. the ssh port) of your pi that's on a local address behind the router.

Finally, some network have computers that reside on both the public and private networks simultaneously. Most simply, you can ssh into the public address of the first computer and then ssh to the private address of the second computer. Alternatively, you can use the intermediate host to make a tunnel.

~~~
$ ssh -A -t [public_userid]@[publichost_address] ssh -A [private_useride]@[privatehost_address]
~~~
{: .language-bash}

{% include links.md %}
