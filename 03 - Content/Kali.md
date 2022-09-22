---
creation date: November 28th 2021
last modified date: November 28th 2021
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]] - [[02 - Internal]]
Links: 
Search Tag: #📖  

# [[Kali]]  
## Setup 

- [ ] Remove transparancy from COOL Kali VM cmd (messes up screenshots)
- [ ] Setup command logging
```bash
# This base64 blob decodes as follows:
# mkdir -p /opt/bashLogs 
# test "$(ps -ocommand= -p $PPID | awk '{print $1}')" == 'script' || (script -f /opt/bashLogs/$(date +"%Y-%m-%d_%H-%M-%S")_shell.log)
# export PS1="[\$(date +\"%Y-%m-%d %H:%M:%S\")] $PS1"
# set -o vi

echo "bWtkaXIgLXAgL29wdC9iYXNoTG9ncyAKdGVzdCAiJChwcyAtb2NvbW1hbmQ9IC1wICRQUElEIHwgYXdrICd7cHJpbnQgJDF9JykiID09ICdzY3JpcHQnIHx8IChzY3JpcHQgLWYgL29wdC9iYXNoTG9ncy8kKGRhdGUgKyIlWS0lbS0lZF8lSC0lTS0lUyIpX3NoZWxsLmxvZykKZXhwb3J0IFBTMT0iW1wkKGRhdGUgK1wiJVktJW0tJWQgJUg6JU06JVNcIildICRQUzEiCnNldCAtbyB2aQo=" |base64 -d >>  ~/.bashrc

echo "termcapinfo xterm* ti@:te@" >> ~/.screenrc

source ~/.bashrc
```

- [ ] Disable the Firewall
```bash 
ufw disable 
```

- [ ] Mount the data share 
![[Share#Mount]]

- [ ] Sync the clock
```bash
apt install ntpdate
ntpdate pool.ntp.org
```


## Alternative APT Sources 
If http://kali.org is blocked, place the following in your `/etc/apt/sources.list` file 

```bash
deb http://mirrors.ocf.berkeley.edu/kali kali-rolling main contrib non-free
# deb-src https://http.kali.org/kali kali-rolling main contrib non-free
```

## Change Resolution 
```bash 
# list resolutions available 
xrandr 

# set resolution 
xrandr -s 1680x1050
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 28th 2021 (11:16 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
