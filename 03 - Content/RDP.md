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

# [[RDP]]  
___

## Description:  

```
rdesktop -u <username> <IP>

rdesktop -d <domain> -u <username> -p <password> <IP>

xfreerdp /u:[domain\]<username> /p:<password> /v:<IP>

xfreerdp /u:[domain\]<username> /pth:<hash> /v:<IP>

```
RDP nmap scripts

`nmap --script "rdp-enum-encryption or rdp-vuln-ms12-020 or rdp-ntlm-info" -p 3389 -T4 <IP>


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:37 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
