Primary Categories: [[01 - Operation]]
Search Tag: #🗺  

# [[02 - Internal]]

## Setup
- [ ] [[Kali#Setup]]
- [ ] [[Share]]  
- [ ] [[Pentest Portal]]
- [ ] Pull Scope from Appendix A to flat files
	- [ ] Create in-scope lists - [[Scope Lists]] 
- [ ] Teamserver Setup (if necessary)

## Enumeration 
- [ ] [[Nessus#Setup]]
- [ ] [[Nmap]] Scans
	- [ ] [[Nmap#Discovery]] Scan 
	- [ ] [[Nmap#Full]] Scan
- [ ] [[Aquatone]] Scans 
- [ ] [[Nessus#Scans]]
- [ ] [[DNSRecon]]


## Initial Access 
- [ ] [[Responder]]
- [ ] [[mitm6.py]]
- [ ] ASREPRoast
- [ ] KerbBrute Spraying
- [ ] PetitPotam to DC with downgrade

#### Total Flow 
- [ ] [[NUC#Setup]]
- [ ] [[Share#Setup]]
- [ ] [[Kali#Setup]]
- [ ] Scope generation (excluding CISA owned assets such as macbooks, VMs, etc)
	- [ ] Pull Scope from Appendix A to flat files
	- [ ] Calculate scope using ~/tools/scoper/Scoper.sh
- [ ] Nessus Setup
- [ ] dnsrecon to enumerate DCs
	- [ ] Check for unrestricted zone transfer
	- [ ] Check for null sessions with rpcclient
- [ ] Network Scans
    - [ ] Discovery
        - [ ] Scan
        - [ ] Review 
    - [ ] Full
        - [ ] Scan
        - [ ] Review
    - [ ] Aquatone
        - [ ] Scan
        - [ ] Review
            - [ ] Printers (Check for Domain Creds)
    - [ ] Nikto
        - [ ] Scan
        - [ ] Review
    - [ ] Nessus
        - [ ] Scan
        - [ ] Review and Validate        
   - [ ] Webapps
       - [ ] Review
   - [ ] Authenticated Scans 
       - [ ] Scan
       - [ ] Review
   - [ ] Database Scans (App Detective)
       - [ ] Scan
       - [ ] Review
- [ ] Gen Relay List
- [ ] Responder
   - [ ] Check NTLMv1 Downgrade
- [ ] Printer Stored Credentials
- [ ] PetitPotam checks
- [ ] CME GPP
- [ ] CME NoPac
- [ ] CME ZeroLogon
- [ ] Log4J
- [ ] kerbrute username enumeration
- [ ] Password Spraying
