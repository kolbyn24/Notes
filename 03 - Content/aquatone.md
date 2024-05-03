---
creation date: September 12th 2022
last modified date: September 12th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Aquatone]]  
___
### Install
Might want to switch to [gowitness](https://github.com/sensepost/gowitness) because aquatone is no longer supported.

Install the latest release from github [aquatone release](https://github.com/michenriksen/aquatone/releases/tag/v1.7.0)
Unzip and then install chrome:
```
sudo apt update

wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo apt install ./google-chrome-stable_current_amd64.deb

sudo apt install chromium

```

## Description:  

Output will end up in the current directory so make a aquatone directory before you run it

[aquatone github](https://github.com/michenriksen/aquatone)

```bash
cat scope_list.txt | aquatone
```

Aquatone can make a report on hosts scanned with the [Nmap](https://nmap.org/) or [Masscan](https://github.com/robertdavidgraham/masscan) portscanner. Simply feed Aquatone the XML output and give it the `-nmap` flag to tell it to parse the input as Nmap/Masscan XML:

```
$ cat scan.xml | aquatone -nmap
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 12th 2022 (02:28 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
