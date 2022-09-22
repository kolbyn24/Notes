---
creation date: November 29th 2021
last modified date: November 29th 2021
aliases: []
tags: #🧰 
---

Primary Categories: [[01 - Operation]] 
Secondary Categories:  [[02 - Internal]]
Links: 
Search Tag: #🧰  

# [[DNSRecon]]  
___

## Description:
DNSRecon can be used early on in an internal engagement to identify nameservers/domain controllers and potentially obtain a list of DNS A records if zone transfers are unrestricted.


## Commands
Locate a domain's nameservers/domain controllers
```
dnsrecon -d domain.local
```

Attempt zone transfers
```
dnsrecon -d domain.local -a
```

You can usually find the FQDN of the network you're connected to by running `ipconfig` from Windows or by checking `/etc/resolv.conf` from Linux


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 29th 2021 (11:36 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
