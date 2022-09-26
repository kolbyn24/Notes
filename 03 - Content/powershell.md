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

disable Defender's Real-time protection (This script contains malicious content and has been blocked by your antivirus software.)

`Set-MpPreference -DisableRealtimeMonitoring $true


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
