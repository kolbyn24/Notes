---
creation date: November 30th 2021
last modified date: November 30th 2021
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  

# [[CrackMapExec]]  

## Kerberos Auth 
For kerberos, use the latest CrackMapExec (>= 5.2.3)
```bash
export KRB5CCNAME=/path/to/ticket.kirbi
cme smb <ip> -d <DOMAIN> -u <USER> -k 
```
## Enumerate Password Policy

```bash
cme --verbose smb -d <DOMAIN> -u <USER> --pass-pol <DC-IP>
```

## Enumerate Machine Account Quota 
```bash 
cme --verbose ldap <dc-ip> -d -u -M maq
```

## Check for NoPac (CVE-2021-42278 | CVE-2021-42287)
```bash
crackmapexec --verbose smb <dc-ip> -u '<user>' -p '<pass>' -M nopac
```

## Scan for WebDAV Systems
```bash 
crackmapexec smb <target-list.txt> -u 'user' -p 'pass' -M webdav
```

## GPP Password 
```bash 
crackmapexec smb <dc-ip> -u 'user' -p 'pass' -M gpp_password
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 30th 2021 (09:54 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>