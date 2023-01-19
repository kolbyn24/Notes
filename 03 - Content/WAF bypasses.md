---
creation date: January 19th 2023
last modified date: January 19th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[WAF bypasses]]  
___

## Description:  

### Tools
[401 bypasses](https://github.com/0ss/byp4ss3r)

### Double encoding method

copy your request to encoder and select "encode as" and then "URL" twice. Paste the encoded payload into your request.

### Using ; in the url

Web application firewall bypass:
 
Adding a ..;/ in the url can trick whitelist and blacklist
 
This gets blocked:
http://hancliffe.htb/maintenance/login.jsp/
 
This doesn’t get blocked:
http://hancliffe.htb/maintenance/..;/login.jsp/



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 19th 2023 (11:14 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
