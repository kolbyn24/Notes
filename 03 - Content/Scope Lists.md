---
creation date: November 30th 2021
last modified date: November 30th 2021
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  

# [[Scope Lists]]  
To create a scope list based on inclusion / exclusion:

```bash 
nmap -sL -n -iL ${include} --excludefile ${exclude}|grep "Nmap scan report for"|cut -d" " -f5 > ${outfile} 2>.Error
```

If this is dying without giving you output, upgrade nmap using apt.

or if you already scaned use this:

```bash
cat all_ips.txt | grep -v -f ./exclude_ips.txt > target_list.txt
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 30th 2021 (09:54 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
