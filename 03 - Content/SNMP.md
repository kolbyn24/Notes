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

# [[SNMP]]  
___

## Description:  

**Banner Grab

`nc -vn <IP> 25


**Scan

`nmap -p25 --script smtp-commands 10.10.10.10

`nmap -p25 --script smtp-ntlm-info 10.10.10.10

`nmap –script smtp-enum-users.nse <IP>


**I was able to get command injection on OpenSMTPD
(you need a valid address in the RCPT TO: field)


```
kolby@kali:~$ nc -vn 192.168.85.71 25
(UNKNOWN) [192.168.85.71] 25 (smtp) open
220 bratarina ESMTP OpenSMTPD
HELO evil-hacker.com
250 bratarina Hello evil-hacker.com [192.168.49.85], pleased to meet you                                                                                                         
250 2.0.0 Ok
RCPT TO: <neil@bratarina>
250 2.1.5 Destination address valid: Recipient ok
DATA
354 Enter mail, end with "." on a line by itself
Subject: Test

This is a test


.
250 2.0.0 21877b0c Message accepted for delivery

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:26 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
