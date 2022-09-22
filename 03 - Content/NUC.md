---
creation date: January 10th 2022
last modified date: January 10th 2022
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  

# [[NUC]]  
Intel NUC device used for onsite assessments 

## Setup 
Always use the bottom port to connect to the network

Discovering the NUC IP
Look for a device with the following ports:
22 
80 
427 
443 
902 
8000 
8300 
9080

```bash
nmap -Pn -n -sS -p 21-23,25,53,111,137,139,445,80,443,8443,8080 --min-hostgroup 255 --min-rtt-timeout 0ms --max-rtt-timeout 100ms --max-retries 1 --max-scan-delay 0 --min-rate 620 -vvv --open 192.168.1.0/24
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 10th 2022 (10:31 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
