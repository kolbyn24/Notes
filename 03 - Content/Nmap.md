---
creation date: April 11th 2022
last modified date: April 11th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]] - [[02 - Internal]]
Links: [[Network Scans]]
Search Tag: #📖  

# [[Nmap]]  
## Discovery 
To discover active hosts, run.
```bash
nmap -Pn -n -sS -p 21-23,25,53,111,137,139,445,80,443,8443,8080,3389 --min-hostgroup 255 --min-rtt-timeout 0ms --max-rtt-timeout 100ms --max-retries 1 --max-scan-delay 0 --min-rate 2000 -vvv --open -iL /PATH/TO/externalip.txt -oA Name-DISC
```

However, this doesnt scan hosts that respond to ICMP, but may have not have one of the above open ports. The following may be helpful if your discovery scans aren't working. Feel free to change the head amount to be how many of the top ports you'd like to check.

```bash
TCP_PORTS=$(echo $(grep /tcp /usr/share/nmap/nmap-services | awk '{print $2}' | cut -d/ -f1 | head -n 100) | sed 's/ /,/g')
UDP_PORTS=$(echo $(grep /udp /usr/share/nmap/nmap-services | awk '{print $2}' | cut -d/ -f1 | head -n 100) | sed 's/ /,/g')
sudo nmap -sn -PP -PA$TCP_PORTS -PS$TCP_PORTS -PU$UDP_PORTS -iL /path/to/externalips.txt  -oA Name-DISC

```

## Full 
When running the full port scan, you'll want to reference the alive hosts from the discover command.
```bash 
nmap -Pn -n -sS -p- -sV --min-hostgroup 255 --min-rtt-timeout 25ms --max-rtt-timeout 100ms --max-retries 1 --max-scan-delay 0 --min-rate 1000 -vvv --open -iL /PATH/TO/discovery/Parsed-Results/Host-Lists/* -oA Name-FULL
```


## Parsing Scans 
Move to the folder where your scan data lives, and run the following command:
```bash 
git clone https://github.com/jasonjfrank/gnmap-parser.git

/tools/gnmap-parser/Gnmap-parser.sh -p
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 11th 2022 (07:08 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
