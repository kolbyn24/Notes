---
creation date: September 22nd 2022
last modified date: September 22nd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[powershell]]  
___

## Description:  

### pscp
For moving files

```
C:\> pscp src des
#push
C:\> pscp root@kail:/path/to/file C:\Path\
#pull
C:\> pscp C:\Path\to\file root@kali:/path/
# -r is for folders
```

### Disable defender

^aec913

disable Defender's Real-time protection (This script contains malicious content and has been blocked by your antivirus software.)

`Set-MpPreference -DisableRealtimeMonitoring $true

`Get-MpPreference` can be used to list the current exclusions.  This can be done locally, or remotely using `remote-exec`.

```
beacon> remote-exec winrm dc-2 Get-MpPreference | select Exclusion*

ExclusionExtension : 
ExclusionIpAddress : 
ExclusionPath : {C:\Shares\software}
ExclusionProcess :
```

If the exclusions are configured via GPO and you can find the corresponding Registry.pol file, you can read them with `Parse-PolFile`.

```
PS C:\Users\Administrator\Desktop> Parse-PolFile .\Registry.pol

KeyName : Software\Policies\Microsoft\Windows Defender\Exclusions
ValueName : Exclusions_Paths
ValueType : REG_DWORD
ValueLength : 4
ValueData : 1

KeyName : Software\Policies\Microsoft\Windows Defender\Exclusions\Paths
ValueName : C:\Windows\Temp
ValueType : REG_SZ
ValueLength : 4
ValueData : 0
```

In a pinch, you can even add your own exclusions.
```
Set-MpPreference -ExclusionPath "<path>"
```

### Windows error codes

```
C:\>net helpmsg 32
The process cannot access the file because it is being used by another process.
```

### Windows Firewalls rules

This can be done with the built-in `netsh` utility. To add an allow rule:
```
netsh advfirewall firewall add rule name="Allow 4444" dir=in action=allow protocol=TCP localport=4444
```

To remove that rule:
```
netsh advfirewall firewall delete rule name="Allow 4444" protocol=TCP localport=4444
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (04:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
