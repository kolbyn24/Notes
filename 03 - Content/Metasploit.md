---
creation date: December 20th 2022
last modified date: December 20th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Metasploit]]  
___

## Description:  

### Basics
View categories
`show -h`
`show auxiliary`

To search for a module
`search portscan`

Use `info` to get more information
`msf5 > info exploit/windows/http/syncbreeze_bof`

To activate a module
`use auxiliary/scanner/portscan/tcp`
or you can search first and then just pick the number you want
`use 4`
`back` can be used to move out of the module and return to the main msf5 prompt.

To view details of a module
`show options`
`set`  and `unset` can be used to set and remove options.

`show payloads` can be used to retrieve a listing of all payloads that are compatible with the currently selected exploit module.

`check` can be used to verify whether or not the target host and application are vulnerable (will only work on certain modules).

use `run` or `exploit` to start the module once the options are set.

We can retrieve information regarding successful login  attempts from the database with `creds`.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 20th 2022 (10:33 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
