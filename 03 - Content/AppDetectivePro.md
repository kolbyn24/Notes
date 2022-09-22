---
creation date: July 7, 2022
last modified date: July 7, 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - Internal]]
Links: 
Search Tag: #📖  
___

## Description:
The client can request up to two database scans. The team conducts these via AppDetectivePro (ADP), which is run from a Windows Host (typically CommandoVM)

## Installation
The tool should already be installed on the CommandoVM provided by the client. 

The Fed Lead should provide license files. Before they do, the Tech Lead should communicate with the fed lead about which SQL server is in use (MSSQL, MySQL, etc). DO NOT trust the customer/PAQ

Be careful to not use up the license as each assessment only gets two licenses and once you scan a database INSTANCE it burns a license.

## Commands
SetUp

![[Pasted Graphic 14.png]]

Configure scan, give it name based on RVA number, configure Asset Name/Host Name based on client input (IP or hostname, etc). configure port/instance info and select the appropriate credential type. 

Often times the client will elect to input the credentials themselves. 

When inputting credentials, if WINDOWS creds are used, do not input hostname, only the domain shortname (unless it's an actual local windows account)

![[Pasted image 20220707103535.png]]

For POLICY, use the CIS Benchmark:
![[Pasted image 20220707103752.png]]

Once creds are tested and policy selected, the license isn't used up until you "Run POLICY"

## Reporting 
Vulnerability Details
	Include Exceptions
	All occurrence types