---
creation date: July 18th 2022
last modified date: Jully 18th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Mangle]]  
___

## Description:
From https://github.com/optiv/Mangle
>Mangle is a tool that manipulates aspects of compiled executables (.exe or DLL). Mangle can remove known Indicators of Compromise (IoC) based strings and replace them with random characters, change the file by inflating the size to avoid EDRs, and can clone code-signing certs from legitimate files. In doing so, Mangle helps loaders evade on-disk and in-memory scanners.

## Installation
```bash
go get github.com/Binject/debug/pe
```

Then build it

```bash
go build Mangle.go
```

## Commands

Example: 

` ./Mangle -C /share/Working/cmd.exe -I /share/Working/payloads/36-PEzor-x64-sgn-unhook-antidebug-text-sleep.exe -O /share/Working/payloads/42-Pezor-Mangle-certificate-inflate.exe -M -S 100`