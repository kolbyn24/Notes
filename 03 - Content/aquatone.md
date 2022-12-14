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

[Aquatone Github](https://github.com/michenriksen/aquatone)

Install instruction on on the github page. Make sure you download the latest release.

## Description:  



```bash
cat scope_list.txt | aquatone
```

You can create a scope list using an include and exclude list [[Scope Lists]]

Or you can use nmap output:

Aquatone can make a report on hosts scanned with the [Nmap](https://nmap.org/) or [Masscan](https://github.com/robertdavidgraham/masscan) portscanner. Simply feed Aquatone the XML output and give it the `-nmap` flag to tell it to parse the input as Nmap/Masscan XML:

```
cat scan.xml | aquatone -nmap
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 12th 2022 (02:28 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
