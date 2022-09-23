---
creation date: September 22nd 2022
last modified date: September 22nd 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Seatbelt]]  
___

## Description:
Seatbelt is a C# project that performs a number of security oriented host-survey "safety checks" relevant from both offensive and defensive security perspectives.

## Installation
Download the source: [Seatbelt](https://github.com/GhostPack/Seatbelt)

 Open **Visual Studio** (not Visual Studio Code), select **Open a project or solution**

Click **OK** to change the target framework to 4.6.1. Then go to **Build > Build Solution**.

## Commands
In CobaltStrike:

`beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Debug\Seatbelt.exe -group=system

To get Proxy Settings

```
beacon> execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Debug\Seatbelt.exe InternetSettings

  HKCU                     ProxyEnable : 1
  HKCU                     ProxyOverride : ;local
  HKCU                     ProxyServer : squid.dev.cyberbotic.io:3128
```






___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 22nd 2022 (06:03 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
