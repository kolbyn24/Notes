Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Post Compromise]]  
{ Links to content pages }

### Post Compromise
Host reconnaissance, Host Persistence, and Host Privilege Escalation for after your initial foothold and you're on the domain.

- [ ] Living off the land
                - Powershell port scanning [[powershell#^1c8090]]
                - 
                - 
- [ ] Host Reconnaissance
                - [[Seatbelt]]
                - screenshot [[CobaltStrike#^563c93]]  [[Metasploit#^5b4372]] 
                - Keylogger [[CobaltStrike#^00b23d]] [[Metasploit#^5b4372]] 
- [ ] Host Persistence
				- [SharPersist](https://github.com/mandiant/SharPersist)
				- Task Scheduler [[Host Persistence#^a718ec]]
                - Startup Folder [[Host Persistence#^007423]]
                - Registry AutoRun[[Host Persistence#^a148a4]]
                - [[COM Hijacks]]
                - Linux host persistence [[bash and linux commands]]
- [ ] Host Privilege Escalation Windows
                - [[Unquoted Service Paths]]
                - [[Weak Service Permissions]]
                - [[Weak Service Binary Permissions]]
                - [[Always Installed Elevated]]
                - [[UAC Bypasses]]
                - [[Windows Privilege Escalation]]
- [ ] Host Privilege Escalation Linux
                - [[Linux Privilege Escalation]]
- [ ] Domain Reconnaissance
				-  [[Active Directory Enumeration#^51dee3]] (some powershell commands)
                - [[Active Directory Enumeration#^aa7159]] (A powershell module)
                - SharpView [[Active Directory Enumeration#^0b6c5b]] (An exe)
                - ADSearch [[Active Directory Enumeration#^2df027]] (An exe but allows ldap queries)
                - [[ldap#^cdfdbf]] (Useful ldap queries)
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
				- [[Process Injection]] for when you don't have creds, but you have local admin privileges. Or if you feel like the service you exploited may be closed by the user.
				- [[SpawnAs]] to have interactive logon for local actions but need creds
				- [[Pass the Hash]] if you have a ntlm hash
				- [[Overpass the Hash]] if you have a NTLM hash or AES keys for a user to request a Kerberos TGT
				- [[Extracting Kerberos Tickets]] extract their TGTs directly from memory if they're logged onto a machine we control without NTLM hash or AES keys
				- [[RubeusToCcache]] - For converting Rubeus base64 format to useful .kirbi and .ccache files. 
				- If you are having trouble getting a shell, try using secrets dump with crackmap or impacket and then PTH with evilrm.
- [ ] [[Session Passing]]
                - For switching from CobaltStrike to Meterpreter
- [ ] Pivoting [cheat sheet](https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/Pivot%20Cheat%20Sheet.pdf)
                - [[SOCKS Proxies]] through CobaltStrike and Metasploit for Kali tools
                - Run GUI Windows Apps through [[Proxifier]]
                - Tunnel Metasploit modules through Beacon's SOCKS proxy by going to **View > Proxy Pivots**, highlighting the existing SOCKS proxy and clicking the **Tunnel** button. paste the command given into msfconsole.
                - To stop the SOCKS proxy, use `socks stop` or **View > Proxy Pivots > Stop**.
                - [[Port Forwards]] to bypass firewall and other network segmentation restrictions.
                - [[NTLM Relaying]] to intercept or capture this authentication traffic in CobaltStrike
                - Pivot Listeners in [[CobaltStrike#^4b657f]]
                - Pivoting in [[Metasploit#^6aac08]]
- [ ] Kerberos Abuse
                - [[Kerberoasting]]
                - [[AS-REP Roasting]]
                - [[Unconstrained Delegation]]
                - [[Printer Bug]]
                - [[Constrained Delgation]]
                - [[S4U2self Absue]]
- [ ] [[Active Directory Certificate Services (AD CS)]]
                - Find Certificate Authorities (CA's) with [[Certify]] (`Certify.exe cas`)
                - Find Vulnerable templates with [[Certify]] (`Certify.exe find /vulnerable`)
                - [[Certipy]] `/home/kali/.local/bin/certipy find -u e.black@coder.htb -p ypOSJXPqlDOxxbQSfEERy300 -dc-ip 10.10.11.207`
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
                - Resource Kit [[Bypassing Antivirus#^4175f5]]
                - AmsiScanBuffer [[Bypassing Antivirus#^7a294d]]
                - Exclusions [[03 - Content/powershell#^aec913]]
                - [[AppLocker Bypasses]]
                - PowerShell Constrained Language Mode [[AppLocker Bypasses#^6d48fa]]
                - [[Frostbyte]]
                - [ProcessHollowing](https://github.com/sbridgens/ProcessHollowing)
                - [[Powershell AV bypasses]]
                - Evil winrm bypass [[Winrm#^93aedf]]
                - [InvisibilityCloak](https://github.com/h4wkst3r/InvisibilityCloak) can bypass av to help you run things like certify.exe
                - [nimcrypt2](https://github.com/icyguider/Nimcrypt2) can be used to obfuscate tools like mimikatz
                - building payloads for exploits [[payloads]]
- [ ] [donPAPI](https://github.com/login-securite/DonPAPI)
- [ ] [[Snaffler]]
- [ ] Data Exfiltration
			- Data Compression in powershell [[powershell#^27344c]]
- [ ] [ICS/SCADA](https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/ICS.md)
- [ ] You can request a tgt with user and password/hash [rubeus](https://github.com/GhostPack/Rubeus)
                