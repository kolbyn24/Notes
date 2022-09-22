Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[External]]  
{ Links to content pages }



### External Access
Identify open ports and services, hosts, operating systems, or existing exploitable vulnerabilities on the network perimeter.

- [ ] External Scanning
                - nmap
                                - Discovery
                                - Full
                - nessus need to create nessus section
                                - Remove sensitive IPs from being scanned
                - Eyewitness / Aquatone
                - 
- [ ] OSINT
                - [[DNS Records]]
	                - dig
	                - whois
	                - [dnscan](https://github.com/rbsec/dnscan)
					- dnsrecon
                                - Zone transfer
                                - Enumerate sub domains
                - [spoofcheck](https://github.com/BishopFox/spoofcheck)
				                -  Weak email security (SPF, DMARC and DKIM) may allow us to spoof emails to appear as though they’re coming from their own domain.
				- shodan
                - theHarvester
- [ ] Password Spraying
                -  Patterns such as MonthYear (August2019), SeasonYear (Summer2019) and DayDate (Tuesday6) are very common.
                - [namemash.py] (https://gist.github.com/superkojiman/11076951) Takes a list of names and transforms it into possible username permutations.
                - [MailSniper](https://github.com/dafthack/MailSniper) Powershell module for password spraying [[MailSniper]]
                - [SprayingToolkit](https://github.com/byt3bl33d3r/SprayingToolkit) Python script for password spraying
                - Go to the exchange server through a browser and login with valid creds
- [ ] File Shares
                - SMB shares
                - ftp
                - NFS
                - [snaffler](https://github.com/SnaffCon/Snaffler)
- [ ] Web Apps
                - Less in depth than a dedicated Web App test. Use the Web Appliation checklist for guidance.
                - Look for default creds
- [ ] SSH / RDP
                - Can you brute force username/passwords?
                - default creds
- [ ] SMTP
- [ ] SNMP
                - snmap walk
- [ ] LDAP
- [ ] MSSQL / mysql / Postgressql
- [ ] Placeholder to update later
