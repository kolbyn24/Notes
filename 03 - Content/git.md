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

### Setting up a .git folder

```
git init master

git status

git add file.sh

git commit -m "First Test"

git log

git diff 4aefc023afb818866bd8c0920d438b44e76f642b a154424d8711ee481cf2e09b8acc6bbc844cddbe > patch.diff
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





___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 6th 2023 (09:09 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
