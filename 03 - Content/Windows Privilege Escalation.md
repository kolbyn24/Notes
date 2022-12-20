---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Windows Privilege Escalation]]  
___

## Description:  
Auto enumeration with [winPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

### useful enumeration
List users
`net user`
More user info
`net user administrator
operating system
`systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"`
running process
`tasklist /SVC`
routing tbales
`route print`
Active network connections
`netstat -ano`
Firewall
`netsh advfirewall show currentprofile`
`netsh advfirewall firewall show rule name=all`
Scheduled Tasks
`schtasks /query /fo LIST /v`
Patches
`wmic product get name, version, vendor`
System Wide Patches
`wmic qfe get Caption, Description, HotFixID, InstalledOn
Readable/Writable Files and Directories
`c:\Tools\privilege_escalation\SysinternalsSuite>accesschk.exe -uws "Everyone" "C:\Program Files"`
Readable/Writable Files and Directories with powershell
`Get-ChildItem "C:\Program Files" -Recurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}`
Unmounted Disks
`mountvol`
Device Drivers and Kernel Modules
`driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object ‘Dis play Name’, ‘Start Mode’, Path`
AlwaysInstallElevated
`reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer`
`reg query HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer`
UAC integrity level
`whoami /groups`




Links to other windows privilege escalation notes:
[[Unquoted Service Paths]]
[[Weak Service Permissions]]
[[Weak Service Binary Permissions]]
[[Always Installed Elevated]]
[[UAC Bypasses]]




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (08:40 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
