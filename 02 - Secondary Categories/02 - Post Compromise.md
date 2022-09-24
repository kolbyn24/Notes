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
				- [[Password Cracking]]
				- [[Make Token]] to impersonate a user with the username, domain and plaintext password for a user
				- [[Process Injection]] for when you don't have creds, but you have local admin privileges
				- [[SpawnAs]] to have interactive logon for local actions but need creds
				- [[Pass the Hash]] if you have a ntlm hash
				- [[Overpass the Hash]] if you have a NTLM hash or AES keys for a user to request a Kerberos TGT
				- [[Extracting Kerberos Tickets]] extract their TGTs directly from memory if they're logged onto a machine we control without  NTLM hash or AES keys
- [ ] [[Session Passing]]
                - For switching from CobaltStrike to Meterpreter