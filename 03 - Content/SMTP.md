---
creation date: May 10th 2023
last modified date: May 10th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[SMTP]]  
___

## Description:  

Set up a mail server with postfix

Or setup a simple one with python:
```
import smtpd
import asyncore

class CustomSMTPServer(smtpd.SMTPServer):

	def process_message(self, peer, mailfrom, rcpttos, data, **kwargs):
		print(f"Received message from {mailfrom} for {rcpttos}:")
		print(data)

server = CustomSMTPServer(('127.0.0.1', 25), None)
asyncore.loop()
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: May 10th 2023 (03:04 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
