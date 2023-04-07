---
creation date: January 7th 2022
last modified date: January 7th 2022
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  , #UnsupportedOS

# [[BloodHound]]  

`sudo apt-get install bloodhound


`sudo neo4j console

navigate to [http://localhost:7474/](http://localhost:7474/) to set up a DB user account by changing default passwords from **neo4j:neo4j** to something else

`sudo bloodhound`

Login with your previously set credentials from neo4j


## SharpHound

[SharpHound](https://github.com/BloodHoundAD/SharpHound)
Running collection methods such as LocalAdmin, RDP, DCOM, PSRemote and LoggedOn will allow SharpHound to enumerate every single computer in the domain. Collecting this information is useful to BloodHound and without it you may see fewer paths, at the obvious expensive of being loud on the wire.


```
beacon> execute-assembly C:\Tools\SharpHound3\SharpHound3\bin\Debug\SharpHound.exe -d cyberbotic.io
```

**for running remotely with python**
[bloodhound.py](https://github.com/fox-it/BloodHound.py)
```
pip install bloodhound


/home/kali/.local/bin/bloodhound-python -u e.black -p ypOSJXPqlDOxxbQSfEERy300 -d coder.htb --collectionmethod All
```

**for cobalt strike**
[[bofhound]]

**Get certificate info**
[Certipy](https://github.com/ly4k/Certipy) `/home/kali/.local/bin/certipy find -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207`

## Queries 

#### Active Unsupported Operating Systems
```cypher
MATCH (n:Computer) 
WHERE n.lastlogontimestamp IS NOT NULL
    MATCH (n) WITH n, datetime({epochSeconds: toInteger(n.lastlogontimestamp)}) as LastLogon 
    WHERE 
    n.enabled AND n.operatingsystem =~ "(?i).*(2000|2003|2008|xp|vista|7|me).*" 
    AND n.lastlogontimestamp IS NOT NULL 
    AND LastLogon.epochseconds > datetime().epochseconds - (90 * 86400)
    AND n.enabled 
RETURN n.name, n.operatingsystem, LastLogon, duration.inDays(date(),LastLogon) as daysSinceLogon
ORDER by daysSinceLogon ASC
```


#### Users not reset password in 90 days

```
MATCH (u:User) WHERE u.pwdlastset < (datetime().epochseconds - (90 * 86400)) and NOT u.pwdlastset IN [-1.0, 0.0] and u.enabled = true and u.lastlogontimestamp is NOT NULL RETURN u.samaccountname as User, datetime({epochSeconds:toInteger(u.lastlogontimestamp)}) as LastLoggedOn, datetime({epochSeconds:toInteger(u.pwdlastset)}) as PasswordLastSet order by datetime({epochSeconds:toInteger(u.lastlogontimestamp)})
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 7th 2022 (07:22 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
