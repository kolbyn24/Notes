Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Internal]]  
{ Links to content pages }



### Internal Access
This testing scenario emulates an adversary with physical access to the internal network but is not yet on the domain.

- [ ] Scanning
                - [[Host Discovery]]
                - [[nmap]]
                                - Discovery
                                - Full
                - nessus (need to create nessus section)
                - [[aquatone]] Scans
                - large networks `masscan 0.0.0.0/0 -p0-65535 –excludedfile exclude.txt`
                - Look for port 88 to be open for the domain controller (also can check for port 636  or 389)

- [ ] [[Responder]]
		- [[scf files]]
- [ ] [[ntlmrelayx.py]]
- [ ] [[NTLM Relaying to ADCS]]
- [ ] [[mitm6.py]]
- [ ] [RITM](https://github.com/Tw1sm/RITM)
- [ ] [[Forcing NTLM Authentication]]
	- [[PetitPotam]]
	- [[Printer Bug]]
	- [SharpSystemTriggers](https://github.com/cube0x0/SharpSystemTriggers)
- [ ] KerbBrute Spraying 
- [ ] [[CrackMapExec]]
- [ ] [[Bloodhound]]
- [ ] [[Shadow Credentials]]
- [ ] Password Spraying
- [ ] [pcrez](https://github.com/lgandx/PCredz) - parse credentials from a pcap or a live interface.


