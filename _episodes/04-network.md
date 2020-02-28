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

## Eduroam

ap_scan=0
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=0
network={
    ssid="eduroam"
    scan_ssid=1
    key_mgmt=WPA-EAP
    eap=TTLS
    anonymous_identity="xxxxxx@umass.edu"
    ca_cert="/boot/eduroam.pem"
    phase2="auth=MSCHAPV2"
    identity="xxxxxxx@umass.edu"
    password="xxxx"
}


{% include links.md %}
