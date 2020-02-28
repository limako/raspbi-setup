---
title: "Network your Pi"
teaching: 0
exercises: 0
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

{% include links.md %}
