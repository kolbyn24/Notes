---
creation date: September 22nd 2022
last modified date: September 22nd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[CobaltStrike]]  
___

## Description:  







### Screenshots

^563c93

Taking screenshots of the user’s desktop can be useful just to see what they are doing. It can show what systems or applications they're using, what shortcuts they have, what documents they’re working on and so on.

```
printscreen               Take a single screenshot via PrintScr method
screenshot                Take a single screenshot
screenwatch               Take periodic screenshots of desktop

```


### KeyLogger

^00b23d

A keylogger can capture the keystrokes of a user, which is especially useful for capturing usernames, passwords and other sensitive data.
`Keylogger`

All keystrokes can be seen at **View > Keystrokes**.


The keylogger runs as a job that can be stopped with the `jobkill` command.

```
beacon> jobs
[*] Jobs

 JID  PID   Description
 ---  ---   -----------
 1    0     keystroke logger

beacon> jobkill 1

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (07:36 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
