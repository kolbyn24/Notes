---
creation date: January 3rd 2022
last modified date: January 3rd 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[Nessus]]
Search Tag: #📖  

# [[Nessus]]  

## External

Nessus environment available via https://nessus0.envXX:8834 where XX is your COOL environment number 

Username: assessment
Password: ask fed lead

### Reports 
When exporting nessus data, always generate the following three artifacts for each scan:
- [ ] Export -> Nessus (.nessus file)
- [ ] Report -> HTML -> Detailed Vulns by Host with Compliance/Remediations 
- [ ] Report -> HTML -> Summary of Hosts with Vulns

## Internal

### Setup 

Install Nessus on one of the op boxes (https://www.tenable.com/downloads/nessus?loginAttempted=true)

```bash 
sudo apt install ~/Downloads/Nessus*_amd64.deb
sudo systemctl enable --now nessusd 
```
- [ ] Open https://localhost:8834/#/
- [ ] Select "Nessus professional"
- [ ] Enter license Key (get from Fed Lead)

username: asmtuser
password: assessment password

### Scans 

Ensure the denial of service and SCADA plugins are disabled before doing any scans 


### block listing
```bash
iptables -I OUTPUT -s xxx.xx.x.xx -j DROP
```

or remove them from the list with

```bash
cat all_ips.txt | grep -v -f ./exclude_ips.txt > target_list.txt
```

### Troubleshooting 

If plugins are not showing up, ensure Nessus is up-to-date with the latest version, and then do the following:

```bash
service nessusd stop
cd /opt/nessus/sbin/
./nessuscli update --plugins-only
# if you get a message about legacy plugin updates not available, restart the service, go to the web console, and in the nessus settings page click "Upgrade to v10", then restart this process
service nessusd start 

# if that doesn't work, you may need to rebuild the plugins database
# Warning: This could take 30-60 minutes

# stop nessus again and run the following to rebuild the plugins database
/opt/nessus/sbin/nessusd -R 

```

#### Certificate woes 

If a custom certificate authority is in use on the network, browse to an inspected / decrypted website and pull the certificate (full chain) to a text file, and place the file in `/opt/nessus/lib/nessus/plugins/custom_CA.inc` and rerun the `./nessuscli update --plugins-only
` command.

### Configuration Changes

Consider changing the port range from "default" to "all"

Don't allow brute force

Uncheck the "Show missing patches that have been superseded" checkbox

Turn on web application scanning

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 3rd 2022 (08:23 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
