---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Active Directory Enumeration]]  
___

## Description:
Tools for domain enumeration:
- Powershell (some powershell commands)
- PowerView (Powershell module)
- SharpView (an exe like PowerView)
- ADsearch (an exe but with ldap queries)
- LDAPSearch (another tool that uses ldap queries)


## Commands

### Powershell

^51dee3

Some enumeration can be done right through Powershell

```
net user /domain

net user jeff_admin /domain

net group /domain

# Get Domain Controllers
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()

# Domain users
Get‐DomainUser | Out‐File ‐FilePath .\DomainUsers.txt

# Properties of a specific user
Get‐DomainUser ‐Identity [username] ‐Properties DisplayName, MemberOf |
Format‐List

```

### PowerView

^aa7159

Download from github here: [PowerSploit](https://github.com/PowerShellMafia/PowerSploit)

In CS beacon:
`beacon> powershell-import C:\Tools\PowerSploit\Recon\PowerView.ps1`

In Powershell:
`PS C:\Tools\active_directory> Import-Module .\PowerView.ps1`

Domain name, the forest name and the domain controllers.
```
beacon> powershell Get-Domain

Forest                  : cyberbotic.io
DomainControllers       : {dc-2.dev.cyberbotic.io}
Children                : {}
DomainMode              : Unknown
DomainModeLevel         : 7
Parent                  : cyberbotic.io
PdcRoleOwner            : dc-2.dev.cyberbotic.io
RidRoleOwner            : dc-2.dev.cyberbotic.io
InfrastructureRoleOwner : dc-2.dev.cyberbotic.io
Name                    : dev.cyberbotic.io
```

Domain controllers for the current or specified domain.
```
beacon> powershell Get-DomainController | select Forest, Name, OSVersion | fl

Forest    : cyberbotic.io
Name      : dc-2.dev.cyberbotic.io
OSVersion : Windows Server 2016 Datacenter

```

Use  `-Forest` to specify a certain forest or use Get-ForestDomain for current forest
```
beacon> powershell Get-ForestDomain

Forest                  : cyberbotic.io
DomainControllers       : {dc-1.cyberbotic.io}
Children                : {dev.cyberbotic.io}
DomainMode              : Unknown
DomainModeLevel         : 7
Parent                  : 
PdcRoleOwner            : dc-1.cyberbotic.io
RidRoleOwner            : dc-1.cyberbotic.io
InfrastructureRoleOwner : dc-1.cyberbotic.io
Name                    : cyberbotic.io

Forest                  : cyberbotic.io
DomainControllers       : {dc-2.dev.cyberbotic.io}
Children                : {}
DomainMode              : Unknown
DomainModeLevel         : 7
Parent                  : cyberbotic.io
PdcRoleOwner            : dc-2.dev.cyberbotic.io
RidRoleOwner            : dc-2.dev.cyberbotic.io
InfrastructureRoleOwner : dc-2.dev.cyberbotic.io
Name                    : dev.cyberbotic.io
```

Returns the default domain policy or the domain controller policy for the current domain or a specified domain/domain controller. Useful for finding information such as the domain password policy.
```
beacon> powershell Get-DomainPolicyData | select -ExpandProperty SystemAccess

MinimumPasswordAge           : 1
MaximumPasswordAge           : 42
MinimumPasswordLength        : 7
PasswordComplexity           : 1
PasswordHistorySize          : 24
LockoutBadCount              : 0
RequireLogonToChangePassword : 0
ForceLogoffWhenHourExpire    : 0
ClearTextPassword            : 0
LSAAnonymousNameLookup       : 0
```

Return all (or specific) user(s).
To only return specific properties, use `-Properties`. By default, all user objects for the current domain are returned, use `-Identity` to return a specific user.
 If you run `Get-DomainUser` without the `-Identity` parameter it will take a long time
```
beacon> powershell Get-DomainUser -Identity nlamb -Properties DisplayName, MemberOf | fl

displayname : Nina Lamb
memberof    : {CN=Roaming Users,CN=Users,DC=dev,DC=cyberbotic,DC=io, CN=Group Policy Creator 
              Owners,CN=Users,DC=dev,DC=cyberbotic,DC=io, CN=Domain Admins,CN=Users,DC=dev,DC=cyberbotic,DC=io, 
              CN=Administrators,CN=Builtin,DC=dev,DC=cyberbotic,DC=io}
```

Return all computers or specific computer objects.
```
beacon> powershell Get-DomainComputer -Properties DnsHostName | sort -Property DnsHostName

dnshostname              
-----------              
dc-2.dev.cyberbotic.io   
nix-1                    
srv-1.dev.cyberbotic.io  
srv-2.dev.cyberbotic.io  
wkstn-1.dev.cyberbotic.io
wkstn-2.dev.cyberbotic.io

```

Search for all organization units (OUs) or specific OU objects.
```
beacon> powershell Get-DomainOU -Properties Name | sort -Property Name

name              
----              
Domain Controllers
Servers           
Tier 1            
Tier 2            
Workstations
```

Return all groups or specific group objects.
```
beacon> powershell Get-DomainGroup | where Name -like "*Admins*" | select SamAccountName

samaccountname
--------------
Domain Admins 
Key Admins    
DnsAdmins     
Oracle Admins

```

Return the members of a specific domain group.
```
beacon> powershell Get-DomainGroupMember -Identity "Domain Admins" | select MemberDistinguishedName

MemberDistinguishedName                             
-----------------------                             
CN=Nina Lamb,CN=Users,DC=dev,DC=cyberbotic,DC=io    
CN=Administrator,CN=Users,DC=dev,DC=cyberbotic,DC=io
```

Return all Group Policy Objects (GPOs) or specific GPO objects. To enumerate all GPOs that are applied to a particular machine, use `-ComputerIdentity`.
```
beacon> powershell Get-DomainGPO -Properties DisplayName | sort -Property DisplayName

displayname                      
-----------                      
Default Domain Controllers Policy
Default Domain Policy
Roaming Users
Tier 1 Admins
Tier 2 Admins
Windows Defender
Windows Firewall

```

```
beacon> powershell Get-DomainGPO -ComputerIdentity wkstn-1 -Properties DisplayName | sort -Property DisplayName

displayname          
-----------          
Default Domain Policy
LAPS
PowerShell Logging
Roaming Users
Windows Defender
Windows Firewall
```

Returns all GPOs that modify local group memberships through Restricted Groups or Group Policy Preferences.
```
beacon> powershell Get-DomainGPOLocalGroup | select GPODisplayName, GroupName

GPODisplayName GroupName           
-------------- ---------           
Tier 1 Admins  DEV\Developers      
Tier 2 Admins  DEV\1st Line Support
```

Enumerates the machines where a specific domain user/group is a member of a specific local group.
```
beacon> powershell Get-DomainGPOUserLocalGroupMapping -LocalGroup Administrators | select ObjectName, GPODisplayName, ContainerName, ComputerName

ObjectName       GPODisplayName ContainerName                                     ComputerName             
----------       -------------- -------------                                     ------------             
1st Line Support Tier 2 Admins  {OU=Tier 2,OU=Servers,DC=dev,DC=cyberbotic,DC=io} {srv-2.dev.cyberbotic.io}
Developers       Tier 1 Admins  {OU=Tier 1,OU=Servers,DC=dev,DC=cyberbotic,DC=io} {srv-1.dev.cyberbotic.io}
```

Enumerates all machines and queries the domain for users of a specified group (default Domain Admins). Then finds domain machines where those users are logged into.
It is very noisy to query every machine in the domain
```
beacon> powershell Find-DomainUserLocation | select UserName, SessionFromName

UserName      SessionFromName          
--------      ---------------          
nlamb         wkstn-2.dev.cyberbotic.io
```

Returns session information for the local (or a remote) machine (where `CName` is the source IP).
```
beacon> powershell Get-NetSession -ComputerName dc-2 | select CName, UserName

CName          UserName
-----          --------
\\10.10.17.231 bfarmer 
\\10.10.17.132 nlamb
```

Return all domain trusts for the current or specified domain.

```
beacon> powershell Get-DomainTrust

SourceName      : dev.cyberbotic.io
TargetName      : cyberbotic.io
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : WITHIN_FOREST
TrustDirection  : Bidirectional
WhenCreated     : 2/19/2021 1:28:00 PM
WhenChanged     : 2/19/2021 1:28:00 PM

SourceName      : dev.cyberbotic.io
TargetName      : subsidiary.external
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : 
TrustDirection  : Inbound
WhenCreated     : 2/19/2021 10:50:56 PM
WhenChanged     : 2/19/2021 10:50:56 PM
```

Logged-in Users on a specific machine
```
PS C:\Tools\active_directory> Get-NetLoggedon -ComputerName client251
```

Active sessions (account or machine logged into another machine)
```
PS C:\Tools\active_directory> Get-NetSession -ComputerName dc01
```



### Sharpview

[SharpView](https://github.com/tevora-threat/SharpView) was designed to be a .NET port of PowerView and therefore has much the same functionality. ^0b6c5b

```
beacon> execute-assembly C:\Tools\SharpView\SharpView\bin\Debug\SharpView.exe Get-Domain

Forest                         : cyberbotic.io
DomainControllers              : {dc-2.dev.cyberbotic.io}
Children                       : {}
DomainMode                     : Unknown
DomainModeLevel                : 7
Parent                         : cyberbotic.io
PdcRoleOwner                   : dc-2.dev.cyberbotic.io
RidRoleOwner                   : dc-2.dev.cyberbotic.io
InfrastructureRoleOwner        : dc-2.dev.cyberbotic.io
Name                           : dev.cyberbotic.io
```

### ADSearch

^6a6097

[ADSearch](https://github.com/tomcarver16/ADSearch) has fewer built-in searches compared to PowerView and SharpView, but it does allow you to specify custom LDAP queries which can be powerful.  Example, finding all domain groups that end in "Admins". ^2df027

```
 "(&(objectCategory=group)(cn=*Admins))"

[*] No domain supplied. This PC's domain will be used instead
[*] LDAP://DC=dev,DC=cyberbotic,DC=io
[*] CUSTOM SEARCH: 
[*] TOTAL NUMBER OF SEARCH RESULTS: 6
[+] cn : Domain Admins
[+] cn : Key Admins
[+] cn : DnsAdmins
[+] cn : Oracle Admins
[+] cn : Subsidiary Admins
[+] cn : MS SQL Admins

```

to look for passwords in the description field of AD
`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Debug\ADSearch.exe --search (&(objectClass=User)(objectCategory=Person)) name,samaccountname,description`


### LDAPSearch

[[ldap#^216b42]]


### Remote Server Administration Tools (RSAT)

If you have admin and GUI access, you can use the built in windows tool.

https://learn.microsoft.com/en-US/troubleshoot/windows-server/system-management-components/remote-server-administration-tools

https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (03:25 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
