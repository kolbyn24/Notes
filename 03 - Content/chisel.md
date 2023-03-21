---
creation date: March 21st 2023
last modified date: March 21st 2023
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[chisel]]  
___

## Description:
Chisel is a fast TCP/UDP tunnel, transported over HTTP, secured via SSH. Single executable including both client and server.

## Installation
[chisel](https://github.com/jpillora/chisel)
`curl https://i.jpillora.com/chisel! | bash`
## Commands

to forward winrm:

your machine
`./chisel server --reverse -p 9002

pivot machine
`./chisel client <your_machine>:9002 R:5985:172.16.22.1:5985


`evil-winrm -i 10.10.14.95 -u 'matthew' -p '147258369'


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 21st 2023 (09:42 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
