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

`apt-get install bloodhound


`neo4j console

navigate to [http://localhost:7474/](http://localhost:7474/) to set up a DB user account by changing default passwords from **neo4j:neo4j** to something else

`bloodhound`

Login with your previously set credentials from neo4j





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

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 7th 2022 (07:22 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
