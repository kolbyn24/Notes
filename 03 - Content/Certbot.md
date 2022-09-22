---
creation date: January 5th 2022
last modified date: January 5th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Certbot]]  
___

## Description:
Certbot is used to request Let's Encrypt SSL certificates 

## Installation
```bash
sudo apt update
sudo apt install snapd

sudo snap install core
sudo snap refresh core 
sudo snap install --classic certbot 

sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## Commands
### Acquire Certificates 
```bash
certbot certonly --standalone -d <FQDN> --agree-tos --register-unsafely-without-email
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 5th 2022 (11:55 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
