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

## Ethernet

The fastest and most reliable want to network your Pi is via ethernet. Connect an ethernet cable with an RJ-45 connector between your Pi and an active ethernet jack. If DHCP service is available, your network connection will be automatically configured and you'll be on the network.

## Eduroam

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

{% include links.md %}
