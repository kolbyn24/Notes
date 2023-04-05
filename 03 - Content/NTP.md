---
creation date: April 5th 2023
last modified date: April 5th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[NTP]]  
___

## Description:  

NTP is UDP so you probably wont see it on your nmap scan. Every DC has it open.

Get a time set:
`enum4linux -a 10.129.215.203`
If it is off you probably have to set your ntp server

### Manually setting a ntp server

Go into your vm settings and turn off time sync. Then run this:
`sudo apt install systemd-timesyncd`
`sudo apt install ntpdate`
`timedatectl set-ntp true`
`timedatectl set-ntp 0`
`sudo ntpdate -u {HTB_DC_IP}`


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 5th 2023 (11:46 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
