Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Post Compromise]]  
{ Links to content pages }

### Post Compromise
Host reconnaissance, Host Persistence, and Host Privilege Escalation for after your initial foothold and you're on the domain.

- [ ] Host Reconnaissance
                - [[Seatbelt]]
                - screenshot [[CobaltStrike#^563c93]] (in CobaltStrike Beacon)
                - Keylogger [[CobaltStrike#^00b23d]] (in CobaltStrike Beacon)
- [ ] Host Persistence
				- [SharPersist](https://github.com/mandiant/SharPersist)
				- Task Scheduler [[Host Persistence#^a718ec]]
                - Startup Folder [[Host Persistence#^007423]]
                - Registry AutoRun[[Host Persistence#^a148a4]]
                - [[COM Hijacks]]
- [ ] Host Privilege Escalation Windows
                - [[Unquoted Service Paths]]
                - [[Weak Service Permissions]]
                - [[Weak Service Binary Permissions]]
                - [[Always Installed Elevated]]
                - [[UAC Bypasses]]
- [ ] Domain Reconnaissance
                - [[PowerView]]
                - SharpView [[PowerView#^0b6c5b]]
                - ADSearch [[PowerView#^2df027]]
                - [[BloodHound]]
- [ ] Lateral Movement
                - Determine if you have local admin access [[CobaltStrike#^7373ce]]
				- [[PowerShell Remoting]]
                - [[PsExec]]
                - [[WMI (Windows Management Instrumentation)]]
                - [[DCOM]]
                - Finding Credentials Locally 
	                - [[Credential Manager]]
					- [[Chrome Passwords]]
				- [[Password Cracking]]
				- [[Make Token]] to impersonate a user with the username, domain and plaintext password for a user
				- [[Process Injection]] for when you don't have creds, but you have local admin privileges
				- [[SpawnAs]] to have interactive logon for local actions but need creds
				- [[Pass the Hash]] if you have a ntlm hash
				- [[Overpass the Hash]] if you have a NTLM hash or AES keys for a user to request a Kerberos TGT
				- [[Extracting Kerberos Tickets]] extract their TGTs directly from memory if they're logged onto a machine we control without NTLM hash or AES keys
- [ ] [[Session Passing]]
                - For switching from CobaltStrike to Meterpreter
- [ ] Pivoting
                - [[SOCKS Proxies]] through CobaltStrike for Kali tools
                - Run GUI Windows Apps through [[Proxifier]]
                - Tunnel Metasploit modules through Beacon's SOCKS proxy by going to **View > Proxy Pivots**, highlighting the existing SOCKS proxy and clicking the **Tunnel** button. paste the command given into msfconsole.
                - To stop the SOCKS proxy, use `socks stop` or **View > Proxy Pivots > Stop**.
                - [[Reverse Port Forwards]] to bypass firewall and other network segmentation restrictions.
                - [[NTLM Relaying]] to intercept or capture this authentication traffic in CobaltStrike
                - Pivot Listeners in [[CobaltStrike#^4b657f]]
- [ ] Kerberos Abuse
                - [[Kerberoasting]]
                - [[AS-REP Roasting]]
                - [[Unconstrained Delegation]]
                - [[Printer Bug]]
                - [[Constrained Delgation]]
                - [[S4U2self Absue]]
- [ ] Active Directory Certificate Services (AD CS)
                - Find Certificate Authorities (CA's) with [[Certify]] (`Certify.exe cas`)
                - Find Vulnerable templates with [[Certify]] (`Certify.exe find /vulnerable`)
                - NTLM Relaying to ADCS HTTP Endpoints ([[NTLM Relaying to ADCS]])
                - [[ADCS for Persistence]]
- [ ] Group Policy Abuse
                - Enumeration [[Group Policy Abuse#^1ad490]]
                - Remote Server Administration Tools (RSAT) (more offsec) [[Group Policy Abuse#^62d4b1]]
                - SharpGPOAbuse (not as offsec but easier) [[Group Policy Abuse#^cfdd15]]
                - 
- [ ] Discretionary Access Control Lists
                - Enumeration [[Discretionary Access Control Lists#^80cd76]]
                - Abuse [[Discretionary Access Control Lists#^7b741a]]
- [ ] MS SQL Server
                - Enumeration [[MS SQL Server#^896209]]
                - MS SQL NetNTLM Capture [[MS SQL Server#^06fa80]]
                - MS SQL Command Execution [[MS SQL Server#^a8479b]]
                - MS SQL Lateral Movement [[MS SQL Server#^db515f]]
                - MS SQL Privilege Escalation [[MS SQL Server#^33c580]]
- [ ] Bypassing Defenses
                - [[Bypassing Antivirus]]
                - Artifact Kit [[Bypassing Antivirus#^0b0bf7]]
                - Resource Kit