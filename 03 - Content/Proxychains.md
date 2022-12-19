---
creation date: August 29th 2022
last modified date: August 29th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Proxychains]]  
___

## Description:  

###CobaltStrike
**In CS beacon:
`socks 5080
 
Before running commands run sleep 0 in beacon to set it to interactive mode. After you run command set it back to sleep 15.

**In kali:

`sudo vim /etc/proxychains4.conf
`socks4 <teamserver_ip> 5080

Comment out: proxy_dns_subnet

**Run commands with

`proxychains -q command

Only tcp goes through proxy (can’t ping)

### SSH

ssh user@ip –D 9050     (sets up dynamic port forward)

`sudo vim /etc/proxychains4.conf
`socks4 127.0.0.1 9050

`Proxychains -q nmap –flags <target_ip> 

This will route all nmap traffic through the socks proxy on the port that is configured in proxychains, this port needs to be the same as used in –D <port> on the ssh declaration. Use -q for quite. Scanning is not the best through proxychains and only tcp traffic will work.

To setup in the browser download foxy proxy. Use a socks4 proxy type, use 127.0.0.1 as the IP, and the port to 9050 (or whatever you set).

To use browser with Burp, set the Browser to the burp proxy and then set burp to use the ssh proxy by editing the SOCKS proxy in the user settings.



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: August 29th 2022 (08:25 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
