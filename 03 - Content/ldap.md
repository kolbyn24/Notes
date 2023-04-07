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

Many tools including ldapsearch will use ldap queries:

`ldapsearch -x -h intelligence.htb -s base namingcontexts` ^216b42

In windows use this:
```
dsquery *  -filter "(samAccountName=e.black)" -attr *
```

ADsearch is another:
[[Active Directory Enumeration#^6a6097]]

### LDAP Queries

^cdfdbf

Get all data (WARNING: this will be very loud and look like bloodhound)
```
ldapsearch (objectclass=*)
```

Retrieve All Schema Info

```
ldapsearch (schemaIDGUID=*) name,schemaidguid -1 "" CN=Schema,CN=Configuration,DC=windomain,DC=local
```

Retrieve Only the ms-Mcs-AdmPwd schemaIDGUID

```
ldapsearch (name=ms-mcs-admpwd) name,schemaidguid 1 "" CN=Schema,CN=Configuration,DC=windomain,DC=local
```

finding all domain groups that end in "Admins"
`"(&(objectCategory=group)(cn=*Admins))"`

to look for passwords in the description field of AD
`beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Debug\ADSearch.exe --search (&(objectClass=User)(objectCategory=Person)) name,samaccountname,description






**How to get hashes from ldap with valid creds
https://github.com/micahvandeusen/gMSADumper

`python3 gMSADumper.py -u 'Ted.Graves' -p 'Mr.Teddy' -d 'intelligence.htb' -l 'dc.intelligence.htb'

If you get a service account hash you can make a [[Silver Ticket#^4540be]]


Also see [[Active Directory Enumeration]]

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (03:19 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
