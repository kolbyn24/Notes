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

In CS beacon:

socks 5080

  
Before running commands run sleep 0 in beacon to set it to interactive mode. After you run command set it back to sleep 15.


  

In kali:

sudo vim /etc/proxychains4.conf

  

socks4 <teamserver_ip> 5080

  

Comment out: proxy_dns_subnet

  
  

Run commands with

proxychains -q command

  

Only tcp goes through proxy (can’t ping)


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: August 29th 2022 (08:25 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
