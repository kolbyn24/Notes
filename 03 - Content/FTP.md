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

# [[FTP]]  
___

## Description:  

Finding Open FTP Servers

Finding FTP servers that allow anonymous logons can assist in numerous red-teaming activities such as Nmap FTP bounce scans.

```
masscan -p 21 <IP Range> -oL ftp_servers.txt; nmap -iL ftp_servers.txt —script ftp-anon -oL open_ftp_servers.txt
```

to connect in terminal:
`ftp 10.10.10.5

type anonymous as the username and password

to connect in browser (did not work with burp)(I dont think this works anymore):

`ftp://10.10.10.5/

`ftp://anonymous:anonymous@10.10.10.5



if FTP allows anonymous logins, do this to download everything:

`wget -m ftp://anonymous:anonymous@192.168.130.128

anonymous

### Simple python ftp server

`python -m pyftpdlib -p21 -w`

If available, use authbind with an unprivileged account
`authbind python -m pyftpdlib -p21 -w`

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (02:01 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
