Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Web Application]]  
{ Links to content pages }



### Web Application
An in-depth web application assessment will be performed against specifically stated assessment target component(s).

- [ ] Enumeration
                - [Basic Enumeration](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web)
                - Manual webpage crawling. Make sure you are proxying traffic through burp
                - gowitness
                - Directory enumeration with dirb or similar tools [[ffuf]]
				                -`feroxbuster --url https://example.com --thorough -o fb_main-1.txt`
				                - To run it authenticated, add something like this to include the session cookies and avoid hitting any logout endpoints(will skip anythig in wordlist that matches pattern): `-H 'Cookie: session=eyJ0...' --dont-scan 'logout'`
				                - By default, it uses `/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt`, but you can specify whatever wordlist you want with `-w`
                - [vhost enumeration](https://sidxparab.gitbook.io/subdomain-enumeration-guide/active-enumeration/vhost-probing)
				                -`gobuster vhost -u http://url/ -t 50 -w subdomains.txt`
				                -`gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u stocker.htb -t 50 --append-domain`
				                -Add sub domain to your hosts files after finding a vhosts
				-Sub domain enumeration
								- `gobuster dns -d mysite.com -t 50 -w subdomains.txt`
								- Other methods here [hacktricks](https://book.hacktricks.xyz/generic-methodologies-and-resources/external-recon-methodology)
                - Look for logins, admin pages, hidden pages, etc.
                - 401 bypasses ( adding a `..;/` can sometimes get by a WAF)
                - Is there a WAF?
                - Can you upload/download files?
                - Error handling
                - Web server identification with [whatweb](https://github.com/urbanadventurer/WhatWeb) (good for getting version numbers)
                - Default web directories 
                                - `C:\inetpub\wwwroot`
                                - `/var/www/ (before Ubuntu 14.04)` 
                                - `/var/www/html/ (Ubuntu 14.04 and later)`
                                - ``/usr/share/nginx/html (nginx)`` 
                                - ``/usr/share/nginx``
- [ ] Scanning
                - Burp suite - Make sure you start scanning from the Target tab. Don't rely on Burp's crawling feature
                - Nikto
                - wpscan
                - Get version numbers and look for exploits
                - [testssl.sh](https://github.com/drwetter/testssl.sh) 
- [ ] Input handling
                - [[XSS]] - 
                                - Make sure to try all inputs for both reflected and stored XXS
                                - [XSS cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
                                - <script>alert(‘XSS’)</script>
                                - Test call backs [webhooks](https://webhook.site/#!/59849c8c-b707-4839-a4b1-d07eb6f8ec26)
                - [[SQLi]] - [[sqlmap]] can help with this
				                - Check input fields
				                - Check URLs
                - [[SSTI]] - [SSTI cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
                - CSV injection
                - [[LFI]] or [[RFI]]
				                - is [[log poisoning]] possible?
                - [OS command injection](https://portswigger.net/web-security/os-command-injection)
                - [HTML injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection) and iframe injection
				                - HTML injection can be used to embed a link to a malicious site
				                - iframe injection can be used for lfi: `<iframe src=/etc/passwd height=500 width=500></iframe>` or `<iframe src=file:///var/www/dev/index.js height=1000px width=1000px></iframe>`
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
                - Does a SOO or auth use a redirect URL? Can you redirect to your own site to steal cookies?
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
- [ ] [[Websockets]] [Web sockets](https://portswigger.net/web-security/websockets) 
- [ ] [[Shellshock]]
- [ ] [[arjun]] (for enumerating http parameters)
- [ ] [[WAF bypasses]]
- [ ] [WebDav](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/put-method-webdav)
- [ ] [Account Takeover](https://www.imperva.com/learn/application-security/account-takeover-ato/)
- [ ] Test call backs [webhooks](https://webhook.site/#!/59849c8c-b707-4839-a4b1-d07eb6f8ec26)
- [ ] Test call backs better but have to have an account [bxsshunter](https://bxsshunter.com/dashboard)


