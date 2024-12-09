Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Phishing]]  
{ Links to content pages }

Good notes to review:
https://gist.github.com/Ridter/c77b1f3cca2dfcee62f5d2ee2236444e
https://book.hacktricks.xyz/generic-methodologies-and-resources/phishing-methodology
https://www.securesystems.de/blog/building-a-red-team-infrastructure-in-2023/

WARNING: be careful about getting your domain or IP burned. Don't leave html pages up, try to test as minimally as possible, use a html obfuscator, use inline images instead of links, block suspicious headers with postfix, and take other steps to stay off of block lists.

Came across this but haven't tried it yet: https://github.com/fin3ss3g0d/evilgophish
### Phishing
Gain foothold into the client network through a phishing email sent with a malicious payload.
- [ ] Buying a domain [namecheap](https://ap.www.namecheap.com/domains/domaincontrolpanel/dc-gov.com/domain)
                - After purchase of domain, go to domain list, click manage on your domain, and set your nameserver to custom DNS. I had 5 nameservers and set them to ns1.linode.com to ns5.linode.com.
                - Make sure to buy a domain as soon as possible, newly registered domains are typically flagged.
- [ ] Creating a publicly available Linode server
                - Create a new Linode server [linode](https://cloud.linode.com/linodes). 
                - go to Domains and click Create > Domain. Leave it as Primary. Put the domain under Domain. Insert default records for the linode you just created.
                - Click on your linode server, and click on network. under the network section, set the reverse DNS to the domain that you purchased (it is the localhost by default).
                - In the firewall section, modify the firewall to allow port 22, 80, 443.
                - In the domains section, create a new domain for the domain name that was purchased.  Add NS records (Name Sever is ns1.linode.com-ns5.linode.com, subdomain is your domain name. There should be 5 lines. )
                - Add A/AAA record. Hostname is purchased domain name and IP Address is your public IP address. 
                - Access the server and setup SSH (ssh root@IP) and setup a user account that can be used for user access (adduser tester).
                - Create a .ssh folder for the tester user (`mkdir .ssh`). Copy the authorized_keys files from root (`cp ~/.ssh/authorized_keys`) change ownership and perms (`chown -R tester:tester .ssh`) (`chmod 700 .ssh`).
                - Add test account to the sudoers group (`usermod -a -G sudo tester`)
                - Add the rest of the team’s SSH keys to the server by adding them to the authorized_keys file (`echo “key” >> authorized_keys`). Verify access (`ssh tester@publicIP`)
                - Check /etc/ssh/sshd.conf and make sure to comment out the lines that allow root login and logging in with a password.
                - ssh onto the server and set up a port forward to the gophish admin page: `ssh -L 3333:127.0.0.1:3333 kali@domain.com`
- [ ] GoPhish server setup [main walkthrough](https://book.hacktricks.xyz/generic-methodologies-and-resources/phishing-methodology) [gophish github](https://github.com/gophish/gophish)
                - Download the Gophish Linux 64-bit Zip file from Releases on GitHub - [gophish_releases](https://github.com/gophish/gophish/releases)
                - Unzip it inside /opt/gophish and execute /opt/gophish/gophish
                - You will be given a password for the admin user in port 3333 in the output. 
                -  Verify that you can hit the admin interface through your localhost (through ssh port forwarding) and login into the application.
                - Stop running the gophish server.
                - TLS certificate configuration – Follow hack tricks article (Be careful when coping and pasting from hacktricks. There are invisible bytes that will mess up a your filename on your keys)
                - Mail configuration – follow hack tricks article
                -  Install snapd and make sure it is running - https://snapcraft.io/docs/installing-snapd [kali_setup_of_snapd](https://snapcraft.io/docs/installing-snap-on-kali)
                - Gophish config – follow hack trick article (skip the wget command)
                - DNS setup can be found in the examples at the bottom of the page.
                - dmarc. Under domains section in linode, add a TXT record. Hostname is `_dmarc`  and value is `v=DMARC1; p=none`
                - dkim. [main walkthrough](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-dkim-with-postfix-on-debian-wheezy). Once that is setup, add a TXT record with the hostname `main._domainkey` and the value of `v=DKIM1; h=sha256; k=rsa; p=MIIBIjANBg...` the value of p comes from `cat mail.txt`. You have to append the second line of the p parameter to the end of the first line and add that to your txt record.
                - spf. Add a TXT record. Hostname is blank, value is `v=spf1 mx a ip4:ip.ip.ip.ip ?all` I.E. `v=spf1 mx a ip4:66.228.36.51 ?all`
                - [Test your email configuration](https://www.mail-tester.com/) you can send emails using `echo "This is the body of the email" | mail -s "This is the subject line" test-iimosa79z@srv1.mail-tester.com` or by sending emails through gophish
                - Edit postfix's header_check.cf to remove suspicious headers: https://www.securesystems.de/blog/building-a-red-team-infrastructure-in-2023/
                - You can test if your IP address or domain name is blacklisted [here](https://mxtoolbox.com/blacklists.aspx)
- [ ] GoPhish campaign configuration
                - Create email template - Make sure to edit through the HTML section. you can use `{{.url}}` to include a link to your landing page.
                - Example email: [[phishing example email]]
                - Create landing page - You can try using the import site feature through gophish but a much better way is using the "save page WE" chrome or firefox extension. This will copy a page to a local .html file. Cat out the file and paste it into gophish. use this [link](https://docs.getgophish.com/user-guide/faq) to troubleshoot form submission not capturing data. Can also use wget to clone a site `wget --mirror --convert-links --adjust-extension --page-requisites --no-parent https://site-to-download.com`
                - [[Multi-Stage Landing Page]] - for logins with multiple steps
                - If needed, training page [[Phishing_training_page]]. Save static page to: /opt/gophish/static/endpoint/ . Then redirect the landing page to https://domain/training.html
                - set up sending profile - host field should be `localhost:25`. Add a custom header with the header "X-Mailer" and the value to a single whitespace. This will remove the X-Mailer: gophish" header.
                - Set up user and group - you can bulk upload by having a .csv file in Firstname,lastname,email,title format. I would also create a test group.
                - Set up campaign - url field should be `https://yourdomain/`. WARNING: if you do not set a launch date and you submit this section it will start sending out emails.
                - To create a static webpage, `mkdir /opt/gophish/static/endpoint` then `echo "test" > index.html`, and then visit https://HOSTNAME/static/index.html 
                - Swap out all links to images with in-line images. This [guide](https://gist.github.com/Ridter/c77b1f3cca2dfcee62f5d2ee2236444e) explains it.
                - Evade signature-based phishing detections (google safe browsing) [article](https://www.r-tec.net/r-tec-blog-evade-signature-based-phishing-detections.html) custom [[html obfuscator]] can be used. 
- [ ] Phishing Payloads
                - [Ivy](https://github.com/optiv/Ivy)
                - [ScareCrow](https://github.com/optiv/ScareCrow)
                - [Mangled](https://github.com/optiv/Mangle)
                - ISO
                                - LNK -> Regasm
                - Custom [[HTA (HTML Application)]]
- [ ] Phishing content/email buildout
- [ ] [[CobaltStrike Setup]]
- [ ] Operating through [[CobaltStrike Setup]]
- [ ] Phishing through teams [TeamsPhisher](https://github.com/Octoberfest7/TeamsPhisher)
- [ ] C2 Obfuscation [cloudfront redirector](https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/Creative%20C2%20Obfuscation%20-%20CloudFronting%20Through%20Firewalls%20and%20Hiding%20in%20Plain%20PCAP.md) [azure](https://github.com/RoseSecurity/Red-Teaming-TTPs/blob/main/Azure%20Static%20Web%20Application%20C2%20Redirectors.md)
- [ ] Google "10min mails addresses" to find temp email addresses to use for testing


### DNS Records
```
#SOA Record
Primary Domain - example.com
Email - example@email.com

#NS Record
Name server - ns1.linode.com
Name server - ns2.linode.com
Name server - ns3.linode.com
Name server - ns4.linode.com
Name server - ns5.linode.com
subdomain - example.com

#MX Record
Mail Server - mail.example.com

#A/AAA record
Hostname - example.com
IP Adress - 1.1.1.1

#TXT record
Hostname - (empty)
Value - v=spf1 mx a ip4:66.228.36.51 ?all
Hostname - _dmarc
Value - v=DMARC1; p=none
Hostname - mail._domainkey
Value - v=DKIM1; h=sha256; k=rsa; p=MIIBIjANBgk....

```

### DNS Zone File Example

```
; example.com [1978497]
$TTL 86400
@  IN  SOA  ns1.linode.com. labs.example.com. 2021000020 14400 14400 1209600 86400
@    NS  ns1.linode.com.
@    NS  ns2.linode.com.
@    NS  ns3.linode.com.
@    NS  ns4.linode.com.
@    NS  ns5.linode.com.
@      MX  10  example.com.
@    300  TXT  "v=spf1 mx a ip4:66.228.36.51 ?all"
mail._domainkey    300  TXT  ( 
  "v=DKIM1; h=sha256; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA+HDO3O/DP7PV6KsLHdDUxcNKvZbG0gUzIvPgAEvzYZDzeWvL/viKSCN4b93lYV4LtNm+hyXfa7YhS6jPuWOpG60EGBuvzlEP6MMDdhbuJq89Kj2UEAeijCVaiXul7WB7au4Om39RN/1aljo1EP4YpgxD6wJoSRXwunLhA7VkeALkSnjz8xOQh/8" 
  "N/PjPIs2eS14I2mM3XeiOT9SqWn6QhGMX7Vy9EW/CbF/CWz4FerUvV05XcSnNsFtSovwq1yVtOy2RdRio0PauZ4m6m5hvNB7iVUXT+YPVCd/kXOEVViOhNPMJ1YAi+DXDzSI/9MJ2L8kafHoK0kn8ZGzBPzWqDwIDAQAB" )
_dmarc    300  TXT  "v=DMARC1; p=none"
@      A  66.228.36.51
adfs    30  A  66.228.36.51
```

### DNS Verification commands
```
# Should show Linode nameservers (ns1.linode.com., ns2.linode.com., etc.) 
dig <domain> +short ns 

# Should respond with the Linode server’s IPv4 address 
dig <domain> +short 

# Should show the subdomain being used for mail (probably mail.<domain>) 
dig <domain> +short mx 

# Whatever the mail subdomain in use, it should resolve to the Linode server’s IP 
dig mail.<domain> +short 

# Should show the SPF record (“v=spf1 mx a ip4:<ip_address> ?all”) 
dig <domain> +short txt 

# Should show the DMARC policy (~v=DMARC1; p=none”) 
dig _dmarc.<domain> +short txt 

# Should show the DKIM info (“v=DKIM1; k=rsa; p=MIIB...”)
dig mail._domainkey.<domain> +short txt 
```