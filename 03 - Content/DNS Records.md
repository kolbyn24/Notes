---
creation date: November 29th 2021
last modified date: November 29th 2021
aliases: []
tags: #🧰 
---

Primary Categories: [[01 - Operation]] 
Secondary Categories:  [[02 - Internal]]
Links: 
Search Tag: #🧰  

# [[DNS Records]]  
___

## Dig
Can provide a list of IPs from a domain name
`dig testdomain.com +short`

then do a whois on each ip
`whois 102.21.90.222

Get all records with:
`dig axfr @10.129.189.81 snoopy.htb`

## DNSRecon

## Description:
DNSRecon can be used early on in an internal engagement to identify nameservers/domain controllers and potentially obtain a list of DNS A records if zone transfers are unrestricted.

## Commands
Locate a domain's nameservers/domain controllers
```
dnsrecon -d domain.local
```

Attempt zone transfers
```
dnsrecon -d domain.local -a
```

You can usually find the FQDN of the network you're connected to by running `ipconfig` from Windows or by checking `/etc/resolv.conf` from Linux

### Making Remote DNS changes with nsupdate

you will need the bind key in order to make updates to the DNS server (If you have an lfi vuln there is a possibility you could grab it)
Config files:
```
/etc/bind/named.conf.local
/etc/bind/named.conf.options
/var/cache/bind/
/etc/named.conf
/etc/rndc.key

name.conf will contain the key shared between servers
/etc/bind/named.conf

key "rndc-key" {
    algorithm hmac-sha256;
    secret "BEqUtce80uhu3TOEGJJaMlSx9WT2pkdeCtzBeDykQQA=";
};
```

Use nsupdate with the bind key to make changes:
```

nsupdate -k /etc/bind/rndc.key <<EOF
server 10.129.189.81
update add mail.snoopy.htb 86400 IN MX 10 mail.snoopy.htb
update add snoopy.htb 86400 IN MX 5 mail.snoopy.htb
update add mail.snoopy.htb 86400 a 10.10.14.114
send
EOF

---or use this

nsupdate -y 'hmac-sha256:rndc-key:BEqUtce80uhu3TOEGJJaMlSx9WT2pkdeCtzBeDykQQA=' <<EOF
server 10.129.189.81
update add mail.snoopy.htb 86400 IN MX 10 mail.snoopy.htb
update add snoopy.htb 86400 IN MX 5 mail.snoopy.htb
update add mail.snoopy.htb 86400 a 10.10.14.114
send
EOF
```

### Bind9 DNS Sever
https://dnns.no/dynamic-dns-with-bind-and-nsupdate.html
```
sudo apt install bind9 bind9utils bind9-doc
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 29th 2021 (11:36 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
