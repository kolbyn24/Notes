---
creation date: November 28th 2021
last modified date: November 28th 2021
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - Deliverables]]
Links: 
Search Tag: #📖  

# [[Share]]  

## Setup 
### Internal
```bash 
ufw status 

# For internal, make sure firewall on share allows connections from your IPs
ufw allow from a.b.c.d/24

# Or if fed lead is ok with it...
ufw disable
```

### External
```bash
# For external, there is no share setup required. However, if the share is not showing up, try the following:
# For windows, just mount \\samba0.env9\share

sudo mount -a
```

### Generating Folder Structure (External)
```bash 
sudo chmod +x /tools/nlzr/folder-structure.sh
sed -i 's/cd \~/cd \//g' /tools/nlzr/folder-structure.sh
/tools/nlzr/folder-structure.sh
mv ~/share/* /share/
```


### On-site Data Extraction (Internal)
```
scp share-archive.zip root@<share-ip>:/mnt/share

ssh root@<share-ip>
cd /mnt/share
unzip share-archive.zip -d /home/share/
```

## Data Structure

## Mount

```bash
mount -ousername=asmtuser -t cifs //<SHARE-IP>/share /mnt/share
```

Make sure firewall on share allows connections (see [[##Setup]] above)

## Share Backup 
Activity Tracker should go into the Documentation folder 
DHS JSON needs to be downloaded for the Fed 
Generate all PTP artifacts including -> backup | report | pptx | dhs json | etc 

## Zipping the Share 
Archive Includes -> Data Documentation Working
Customer Archive Includes -> Data Documentation 
Ensure that PTP backup zip is on /share/ptp/backup*
```bash
cd /mnt/share 
zip -r STATE-SHORTNAME_RV####_YYYYMMDD.zip
```

## Deliverable Checklist 
- [ ] Cobalt Strike Zip 

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 28th 2021 (11:51 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
