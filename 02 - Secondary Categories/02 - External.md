Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[External]]  
{ Links to content pages }



### External Access
Identify open ports and services, hosts, operating systems, or existing exploitable vulnerabilities on the network perimeter.

- [ ] [[DNS Enumeration]]
- [ ] External Scanning - test 
                - nmap
                                - Discovery
                                - Full
                - nessus need to create nessus section
                                - Remove sensitive IPs from being scanned
                - Eyewitness / Aquatone - [gowitness](https://github.com/sensepost/gowitness) is the new project used for this.
                - 
- [ ] OSINT
				- Find unknown server
					- Use crt.sh to identify domains
					- Use Shodan with `ssl.cert.subject.cn:<sub.domain.com>` and then `ssl.cert.subject.cn:<sub.domain.com> "HTTP/"
					- Look for CVEs shodan identified
					- recon-ng can use the api from crt.sh to look for tls certificates on webservers to find alternate names.
					- [full shodan guide](https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/Shodan_Guide.pdf)
                - [[DNS Records]]
	                - dig
	                - whois
	                - [dnscan](https://github.com/rbsec/dnscan)
					- dnsrecon
                                - Zone transfer
                                - Enumerate sub domains
                - [spoofcheck](https://github.com/BishopFox/spoofcheck)
				                -  Weak email security (SPF, DMARC and DKIM) may allow us to spoof emails to appear as though they’re coming from their own domain.
				- Good [hacktricks](https://book.hacktricks.xyz/generic-methodologies-and-resources/external-recon-methodology)
- [ ] Password Spraying
                -  Patterns such as MonthYear (August2019), SeasonYear (Summer2019) and DayDate (Tuesday6) are very common.
                - [namemash.py] (https://gist.github.com/superkojiman/11076951) Takes a list of names and transforms it into possible username permutations.
                - [MailSniper](https://github.com/dafthack/MailSniper) Powershell module for password spraying [[MailSniper]]
                - [SprayingToolkit](https://github.com/byt3bl33d3r/SprayingToolkit) Python script for password spraying
                - Go to the exchange server through a browser and login with valid creds
- [ ] [MFASweep](https://github.com/dafthack/MFASweep) - attempts to log in to various Microsoft services using a provided set of credentials
- [ ] File Shares
                - [[SMB]] shares
                - [[FTP]]
                - [[nfs]]
                - [snaffler](https://github.com/SnaffCon/Snaffler)
                - [[scf files]] to stead password hashes.
- [ ] Web Apps
                - Less in depth than a dedicated Web App test. Use the Web Application checklist for guidance.
                - Look for default creds
- [ ] [[SSH]] / [[RDP]]
                - Can you brute force username/passwords? [[Hydra]]
                - default creds
- [ ] [[SMTP]]
- [ ] [[NTP]]
- [ ] [[SNMP]]
                - snmap walk
- [ ] [[ldap]]
- [ ] MSSQL / [[mysql]] / Postgressql
- [ ] [[Firewall Enumeration]]
- [ ] [[Winrm]] port 5985
- [ ] Client-Side Attacks
				- [[HTA (HTML Application)]] (Needs to be opened through IE)
				- [[VBA Macro]] (Microsoft office files)
				- [[Object Linking and Embedding]] (embedded in a word doc)
				- [[HTML Smuggling]] (Webpage that will automatically download an executable, can sometimes bypass proxy filtering)
				- [[Windows Script Host]] (Jscript .js files)
				- [[Process Injection in Csharp]]
				- [[DLL Injection]]
				- [[Process Hollowing]]
				- [[Frostbyte]]
- [ ] Brute Force
				- [[CrackMapExec]]
				- [[Hydra]]
				- [[Crowbar]] for RDP
- [ ] Sub Domain takeover
				- Identify dns records that have a cname that points to nothing (dangling DNS record).
				- https://book.hacktricks.xyz/pentesting-web/domain-subdomain-takeover
				- https://github.com/EdOverflow/can-i-take-over-xyz
				- Can automate this with [tko-subs](https://github.com/anshumanbh/tko-subs) (`sudo go install -v github.com/anshumanbh/tko-subs@latest`) (gets download in ~/go/bin)
					- `./tko-subs -domain ferc.gov -data=providers-data.csv` (need to download csv from github page)
