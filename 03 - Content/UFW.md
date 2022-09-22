---
creation date: January 6th 2022
last modified date: January 6th 2022
aliases: []
tags: #🧰
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: 
Search Tag: #🧰   - #Firewall

# [[UFW]]  
___

## Description:
Keep out the crawlies 

## Installation
```bash 
apt install ufw
```

### Cloudfront
Include these IPs if using cloudfront 

https://d7uri8nf7uskq.cloudfront.net/tools/list-cloudfront-ips

```bash 
# block microsoft safesender / atp inspection at teamserver
curl -s https://endpoints.office.com/endpoints/worldwide?clientrequestid=b10c5ed1-bad1-445f-b386-b919946339a7 | grep '/' | tr -s ' ' | cut -f 2 -d ' ' | tr -d '"' | tr -d ',' > protection-ips.txt 

# allowable cloudfront ips
wget https://d7uri8nf7uskq.cloudfront.net/tools/list-cloudfront-ips
cat list-cloudfront-ips | tr ',' '\n' | tr '"' ' ' | tr '[' '\n' | tr -s ' ' | cut -f 2 -d ' ' | grep -v CLOUDFRONT > cloudfront-ips.txt
```

## Commands
Run the below on all machines, paying attention to the comments towards the bottom of the file 

```bash 
#!/bin/bash

ufw enable 

ufw status numbered

rules=$(ufw status numbered | wc -l)
rule_count=$((rules-5))
for ((i=rule_count; i>0; i--));do echo "Deleting $i" && echo "yes" | ufw delete $i;done

ufw allow from 10.0.0.0/8 to any

cat /share/Working/scope/external.txt /share/Working/infrastructure/cisa-ips-only.txt > /tmp/firewallchanges
while read line; do ufw allow proto tcp from $line to any port 80,443,5000:5999; done < /tmp/firewallchanges
rm /tmp/firewallchanges

# if teamserver, uncomment below line 
# ufw allow 53/udp

# if mail server, uncomment below lines
# ufw allow proto tcp from any to 25,587

ufw status numbered
```

**_IPs.txt_**
```
1.1.1.1
2.2.2.2
3.3.3.3
```

**_Whitelist IPs to 443_**
```bash
while read line; do ufw allow proto tcp from $line to any port 80,443; done < IPs.txt
```

**_Remove access to 443_**
```bash
while read line; do ufw delete allow proto tcp from $line to any port 80,443; done < IPs.txt
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 6th 2022 (09:47 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
