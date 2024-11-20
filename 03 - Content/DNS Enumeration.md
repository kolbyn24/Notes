---
creation date: November 20th 2024
last modified date: November 20th 2024
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[DNS Enumeration]]  
___

## Description:  

The goal of DNS enumeration is to take a top level domain and find either sub-domains or records containing CNAMES that point to either interesting servers or domains that can be taken over (DNS name that can be purchased or sub-domain takeover with some sort of cloud environment)

### Tools:

cert.sh - website that checks DNS records
wayback urls - local tool that uses the wayback machine to find old records based on a domain name
brute_subs.sh - Patrick Higgins wrote this script to automate subdomain brute forcing
owasp amass - github page that uses other techniques to find subdomains

------

### Process:
Set the top level domain (bash):
```
export DOMAIN=example.com
```






___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 20th 2024 (03:37 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
