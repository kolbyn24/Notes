---
creation date: January 10th 2022
last modified date: January 10th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - Internal]] - [[02 - External]]
Links: [[Nmap]] - [[aquatone]] - 
Search Tag: #📖  

# [[Network Scans]]  

## Renaming Raw Data 
```bash 
split --numeric-suffixes=01 -n l/10 --additional-suffix '-hosts.txt' internal-scans.txt 'SHORTORG-'
```

## List of /24s for Large networks
```bash 
cat RVXXXX-PingSweep.gnmap | grep "Status: Up" | cut -d ' ' -f 2 > LiveIPs.txt
cat LiveIPs.txt | cut -d "." -f 1,2,3 | uniq | xargs -I {} echo {}.0/24 > Live24s.txt
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 10th 2022 (12:08 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
