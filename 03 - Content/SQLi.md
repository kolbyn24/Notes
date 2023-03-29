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

# [[SQLi]]  
___

## Description:  



### URL
Look for sql statements like the following:

`http://kioptrix3.com/gallery/gallery.php?id=1&sort=size#photos

This caused a sql error to be returned
`http://kioptrix3.com/gallery/gallery.php?id='

### NoSQL databases

If the backend is a NoSQL database, it is possible to change the content type from `Content-Type: application/x-www-form-urlencoded` to `Content-Type: application/json` to attempt sql injection:
from:
`username=admin&password=admin`
to:
`{"username":{"$ne":"admin"}, "password":{"$ne":"pass"}}`

### websockets
[[Websockets]]



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:06 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
