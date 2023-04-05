---
creation date: April 5th 2023
last modified date: April 5th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[ffuf]]  
___

## Description:  

[github](https://github.com/ffuf/ffuf)
[good how to](https://codingo.io/tools/ffuf/bounty/2020/09/17/everything-you-need-to-know-about-ffuf.html#recursion)

### Using fuff to enumerate a post request parameter using a input file

`ffuf -w number.txt -X POST -d "password=FUZZ" -b "TCSESSIONID=74BCFD74A212BEC7DF8533FAD3E6FA50" -request ../request.txt -u https://teamcity-dev.coder.htb/2fa.html -fr "Incorrect" -c `


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: April 5th 2023 (06:58 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
