---
creation date: September 26th 2022
last modified date: September 26th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Forged Certificates]]  
___

## Description:  

In larger organisations, the AD CS roles are installed on separate servers and not on the domain controllers themselves.  Often times, they are also not treated with the same sensitivity as DCs.  So whereas only EAs and DAs can access/manage DCs, "lower level" roles such as server admins can often access the CAs.

So although this can be also seen a privilege escalation, it's just as useful as a domain persistence method.

Gaining local admin access to a CA allows an attacker to extract the CA private key, which can be used to sign a forged certificate (think of this like the krbtgt hash being able to sign a forged TGT).  The default validity period for a CA private key is 5 years, but this can obviously be set to any value during setup, sometimes as high as 10+ years.


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:09 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
