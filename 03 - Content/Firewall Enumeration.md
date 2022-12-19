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

# [[Firewall Enumeration]]  
___

## Description:  

Use netcat to try to connect out through every port and view the responses in tcpdump

```
On victim:
nc -zv 192.168.19.55 1-65535
On Kali:
tcpdump -I tun0 src host 192.168.55.89
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (02:28 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
