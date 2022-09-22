---
creation date: January 27th 2022
last modified date: January 27th 2022
aliases: []
tags: #🧰 
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[Internal]]
Links:
Search Tag: #📖  

# [[SeeYouCMThief]]  
## Description:
SeeYouCMThief can be used to find Cisco phone configuration files and parse them for SSH credentails. Identified SSH credentials are commonly resued on the CUCM (Cisco Unified Communications Manager) web interface. If an attack gains access to the CUCM web interface, it is possible to dump AD creds from configured LDAP authentication settings, comparable to the attack commonly targeting printers.

## Installation
```shell
git clone https://github.com/trustedsec/SeeYouCM-Thief
pip3 install -r requirements.txt
```

## Commands
If the CUCM server exposes *ConfigFileCacheList.txt*, the tool won't need to gather a list of Cisco phone hostnames. In this case it's fine to provide the IP of the CUCM server directly if you know it, or a single Cisco phone IP you've found. If the CUCM server doesn't expose that CacheList file, you may be better off having the tool enumerate as many phone hostnames as possible, which should lead to more config files to download and parse.

Provide the address of the CUCM server directly:
```shell
./thief.py -H 10.10.1.10
```

Provide the address of a single Cisco phone:
```shell
./thief.py --phone 10.10.2.100
```

Provide an entire subnet to scan for Cisco phones with 80 open:
```shell
./thief.py --enumsubnet 10.10.2.0/24
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |
| [Blog](https://www.trustedsec.com/blog/seeyoucm-thief-exploiting-common-misconfigurations-in-cisco-phone-systems/) | TrustedSec Blog |
| [Git Repo](https://github.com/trustedsec/SeeYouCM-Thief) | GitHub Repository |

Created Date: January 27th 2022 (11:08 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
