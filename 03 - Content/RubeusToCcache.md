---
creation date: August 10th 2023
last modified date: August 10th 2023
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[RubeusToCcache]]  
___
Github Page
[RubeusToCcache](https://github.com/SolomonSklash/RubeusToCcache)
## Description:
For converting Rubeus base64 format to useful .kirbi and .ccache files. 

## Installation
```
git clone https://github.com/SolomonSklash/RubeusToCcache
cd RubeusToCcache
pip3 install -r requirements.txt
rubeustoccache.py [-h] base64_input kirbi ccache
```

## Commands
Use `Rubeus dump /nowrap` to export base64 encode ticket.

Copy that string to a file and use Linux to trim the whitespace
```
cat testing.txt | tr -d " \t\n\r"
```

Now use that string to convert it to useable files:

```
rubeustoccache.py <base64_string> user.kirbi user.ccache
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: August 10th 2023 (12:48 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
