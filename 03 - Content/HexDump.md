---
creation date: January 3rd 2022
last modified date: January 3rd 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - Internal]] - [[02 - External]]
Links: [[Shellcode]]
Search Tag: #📖  

# [[HexDump]]  
## Bin to C\#

```bash
# one file 
hexdump -v -e '1/1 "0x%02x,"' shellcode.bin | sed 's/.$//' > shellcode.cs

# all files 
ls *.bin | xargs -I {} /bin/bash -c "hexdump -v -e '1/1 \"0x%02x,\"' {} | sed 's/.$//' > {}.cs"
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 3rd 2022 (10:17 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
