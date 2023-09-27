---
creation date: September 27th 2023
last modified date: September 27th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Host Discovery]]  
___

## Description:  

[host discovery hacktricks](https://book.hacktricks.xyz/generic-methodologies-and-resources/pentesting-network)

### Pinging
```
Broadcast ping: 
ping -b 10.4.1.255

Ping other networks:
ping -b 255.255.255.255

Scan the local network, using the information from the primary network interface:
arp-scan -l

Scan a subnet, specifying the interface to use and a custom source MAC address:
arp-scan -I eth0 --srcaddr=DE:AD:BE:EF:CA:FE 192.168.86.0/24

Fingerprint a system using ARP:
arp-fingerprint -h

```


### bettercap
https://www.geeksforgeeks.org/sniffing-using-bettercap-in-linux/amp/
```
bettercap  -iface wlan0
net.show
net.probe on
help
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.43.157(IP address of the target Device)
set arp.spoof on
set net.sniff.local true
net.sniff on
```

### Wifi hacking with bettercap

https://null-byte.wonderhowto.com/how-to/hack-wi-fi-networks-with-bettercap-0194422/

```
# need a monitor mode-compatible adapter.
airmon-ng start wlan1

sudo bettercap --iface wlan1mon
help wifi
wifi.recon on
events.stream off
wifi.show
wifi.deauth all
set wifi.handshakes '/desiredfolderlocation'
wifi.assoc all
#Check file created for a hash to brute force with hashcat

```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 27th 2023 (10:00 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
