---
creation date: May 26th 2022
last modified date: May 26th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - Internal]]
Links: [[RBCD]]
Search Tag: #📖  

# [[Resource Based Constrained Delegation]]  

### Check Current RBCD Values
```bash
rbcd.py -delegate-to 'computerName$' -dc-ip '<DC-IP>' -action read 'domain.local'/'username':'password'
```

### Remove RBCD Entry 
```bash
rbcd.py -delegate-to 'computerName$' -dc-ip '<DC-IP>' -action remove -delegate-from '<OWNED-COMPUTER>$' -k -no-pass 'domain.local/privelegedusername:password'
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: May 26th 2022 (08:32 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
