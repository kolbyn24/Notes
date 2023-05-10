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

Use this one to display the emails as html (without any encoding), which will fix broken links:
```
import smtpd
import asyncore
from email.message import EmailMessage
from email.parser import BytesParser
from email.policy import default

class CustomSMTPServer(smtpd.SMTPServer):

	def process_message(self, peer, mailfrom, rcpttos, data, **kwargs):
		message = BytesParser(policy=default).parsebytes(data)

		html = None
		for part in message.walk():
			if part.get_content_type() == 'text/html':
				html = part.get_payload(decode=True).decode('utf-8')
				break

		print(f"Received message from {mailfrom} for {rcpttos}:")
		print(html)

server = CustomSMTPServer(('0.0.0.0', 25), None)
asyncore.loop()

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: May 10th 2023 (03:04 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
