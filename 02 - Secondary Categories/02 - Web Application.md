Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Web Application]]  
{ Links to content pages }



### Web Application
An in-depth web application assessment will be performed against specifically stated assessment target component(s).

- [ ] Enumeration
                -  Manual webpage crawling. Make sure you are proxying traffic through burp
                - Directory enumeration with dirb or similar tools
                - Look for logins, admin pages, hidden pages, etc.
                - 401 bypasses ( adding a `..;/` can sometimes get by a WAF)
                - Is there a WAF?
                - Can you upload/download files?
                - Error handling
                - Default web directories 
                                - `C:\inetpub\wwwroot`
                                - `/var/www/ (before Ubuntu 14.04)` 
                                - `/var/www/html/ (Ubuntu 14.04 and later)`
- [ ] Scanning
                - Burp suite - Make sure you start scanning from the Target tab. Don't rely on Burp's crawling feature
                - Nikto
                - wpscan
                - Get version numbers and look for exploits
                - [testssl.sh](https://github.com/drwetter/testssl.sh) 
- [ ] Input handling
                - XXS - 
                                - Make sure to try all inputs for both reflected and stored XXS
                                - [XXS cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
                                - <script>alert(‘XSS’)</script>
                - [[SQLi]] - [[sqlmap]] can help with this
				                - Check input fields
				                - Check URLs
                - [[SSTI]] - [SSTI cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
                - CSV injection
                - [[LFI]] or [[RFI]]
				                - is [[log poisoning]] possible?
                - [OS command injection](https://portswigger.net/web-security/os-command-injection)
- [ ] [XML External Entities](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing)
                - Look for any POST or PUT requests using XML
                - If the app is only using JSON, try a JSON to XML conversation [XXE on JSON](https://www.netspi.com/blog/technical/web-application-penetration-testing/playing-content-type-xxe-json-endpoints/)
- [ ] Registration
                - Can you overwrite existing user [Takeover Vulnerabilities](https://book.hacktricks.xyz/pentesting-web/registration-vulnerabilities)
                - Can you Enumerate valid users 
                                - Was there a folder created for a new user?
- [ ] [Authenication](https://portswigger.net/web-security/authentication)
                - Proxy traffic through burp, look through requests and responses. 
                                - Look for permissions being set in the response. Can you replace username with another in response? Can you bypass authentication?
                                - Is it in JSON? Can you add multiple username and passwords?
                - Can you enumerate usernames? What is the lockout policy?
                - Weak password policy?
                                - Can passwords be short or not complex
                                - Can dictionary words be used
                                - Can spaces be used
                                - Are extremely long passwords allowed (DOS?)
                - Use [[Hydra]] or Burp to try to brute force the login.
- [ ] Session Management
                - What is the session cookie? Remove cookies with repeater until it logs you out.
                - [auth analyzer](https://portswigger.net/bappstore/7db49799266c4f85866f54d9eab82c89) burp extension is really good at finding auth issues
                - Do session cookies expire after a peroid of time and after manual log out
                - check for httponly and secure flags
                - Try privileged actions with an unprivileged user cookie
                - Find parameter with user id and try to tamper in order to get the details of other users
- [ ] Application Security Verification Standard [ASVS](https://github.com/OWASP/ASVS)
                - Go through the checklist and note any differences in the standard. 
- [ ] Mobile Application Security Verification Standard [MASVS](https://github.com/OWASP/owasp-masvs)
                - Go through the checklist and note any differences in the standard. 
- [ ] [http request-smuggling](https://portswigger.net/web-security/request-smuggling)
- [ ] [Web sockets](https://portswigger.net/web-security/websockets)
- [ ] [[Shellshock]]
- [ ] [[arjun]] (for enumerating http parameters)
- [ ] [[WAF bypasses]]



