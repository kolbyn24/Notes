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
Use crt.sh
```
#This command will give you the raw HTML respnse
curl "https://crt.sh/?q=$DOMAIN" -o "${DOMAIN}_crt_sh.txt"
#This command will sort the HTML and give you a clean list of sub domains
grep '<TD>' "${DOMAIN}_crt_sh.txt" | grep -Po "[^>]+$DOMAIN" | sort -u > "${DOMAIN}_crt_sh_sorted.txt"
```
You can attempt to resolve these either by opening them in firefox or using eyewitness.
```
# Check the number of lines, be careful opening large files in firefox
wc -l "${DOMAIN}_crt_sh_sorted.txt"
# Open in Firefox
cat "${DOMAIN}_crt_sh_sorted.txt" | xargs firefox
# Take screenshots with eyewitness
./EyeWitness -f "${DOMAIN}_crt_sh_sorted.txt" --web
```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 20th 2024 (03:37 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
