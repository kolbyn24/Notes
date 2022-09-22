---
creation date: June 21st 2022
last modified date: June 21st 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[Phishing]]
Links: 
Search Tag: #📖  

# [[onmicrosoft.com Domains]]  
Office365 domains that use a 3rd-party mail filter (such as Proofpoint or Mimecast) can sometimes have mail sent directly to O365, in a way that bypasses the 3rd-party. If not correctly configured, email sent to the targets `onmicrosoft.com` or `mail.onmicrosoft.com` domains can end up in inboxes without inspection from the filter (they will instead be inspected by the filter at Office365). 

If the target's MX record already points to an Office365 host (`*.mail.protection.outlook.com`) this technique will not have any effect.

There is not a surefire way to determine a target domain's onmicrosoft.com address. Occasionally it will be leaked in email headers from the target's domain (can ask Fed lead to search through the headers of prior correspondence) or it can frequently be guessed. 

The `dig` command can be used for guess attempts. A successful guess will contain `mail.protection.outlook.com` in the ANSWER SECTION, as shown below:

```bash
-$ dig mx CLIENT.onmicrosoft.com

; <<>> DiG 9.18.1-1-Debian <<>> mx CLIENT.onmicrosoft.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3913
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;CLIENT.onmicrosoft.com.                IN      MX

;; ANSWER SECTION:
CLIENT.onmicrosoft.com. 3600    IN      MX      0 CLIENT.mail.protection.outlook.com.

;; Query time: 15 msec
;; SERVER: 10.224.160.2#53(10.224.160.2) (UDP)
;; WHEN: Tue Jun 21 19:11:44 UTC 2022
;; MSG SIZE  rcvd: 98

```

A failure will show not `mail.protection.outlook.com` in the results in the ANSWER SECTION and it will usually be replaced with entries containing `azuredns`:
```bash
-$ dig mx CLIENT.onmicrosoft.com                                                                                                  

; <<>> DiG 9.18.1-1-Debian <<>> mx CLIENT.onmicrosoft.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 34559
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;CLIENT.onmicrosoft.com.       IN      MX

;; AUTHORITY SECTION:
onmicrosoft.com.        235     IN      SOA     ns1-208.azure-dns.com. azuredns-hostmaster.microsoft.com. 1 3600 300 2419200 300

;; Query time: 15 msec
;; SERVER: 10.224.160.2#53(10.224.160.2) (UDP)
;; WHEN: Tue Jun 21 19:15:52 UTC 2022
;; MSG SIZE  rcvd: 136

```

It is recommended to try both `.onmicrosoft.com` and `.mail.onmicrosoft.com`
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |
|[Pracitcal365](https://practical365.com/how-to-ensure-your-third-party-filtering-gateway-is-secure/)|Blog|

Created Date: November 28th 2021 (11:51 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
