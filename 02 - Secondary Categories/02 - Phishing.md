Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Search Tag: #🗺  

# [[Phishing]]  
{ Links to content pages }


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
                - ssh onto the server and set up a port forward to the gophish admin page: `ssh -L 3333:127.0.0.1:3333 kali@domain.com`
- [ ] GoPhish server setup [main walkthrough](https://book.hacktricks.xyz/generic-methodologies-and-resources/phishing-methodology) [gophish github](https://github.com/gophish/gophish)
                - Download the Gophish Linux 64-bit Zip file from Releases on GitHub - [gophish_releases](https://github.com/gophish/gophish/releases)
                - Unzip it inside /opt/gophish and execute /opt/gophish/gophish
                - You will be given a password for the admin user in port 3333 in the output. 
                -  Verify that you can hit the admin interface through your localhost (through ssh port forwarding) and login into the application.
                - Stop running the gophish server.
                - TLS certificate configuration – Follow hack tricks article (Be careful when coping and pasting from hacktricks. There are invisible bytes that will mess up a your filename on your keys)
                - Mail configuration – follow hack tricks article
                -  Install snapd and make sure it is running - https://snapcraft.io/docs/installing-snapd 
                - Gophish config – follow hack trick article (skip the wget command)
                - dmarc. Under domains section in linode, add a TXT record. Hostname is `_dmarc`  and value is `v=DMARC1; p=none`
                - dkim. [main walkthrough](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-dkim-with-postfix-on-debian-wheezy). Once that is setup, add a TXT record with the hostname `main._domainkey` and the value of `v=DKIM1; h=sha256; k=rsa; p=MIIBIjANBg...` the value of p comes from `cat mail.txt`. You have to append the second line of the p parameter to the end of the first line and add that to your txt record.
                - spf. Add a TXT record. Hostname is blank, value is `v=spf1 mx a ip4:ip.ip.ip.ip ?all` I.E. `v=spf1 mx a ip4:66.228.36.51 ?all`
                - [Test your email configuration](https://www.mail-tester.com/) you can send emails using `echo "This is the body of the email" | mail -s "This is the subject line" test-iimosa79z@srv1.mail-tester.com` or by sending emails through gophish
                - You can test if your IP address or domain name is blacklisted [here](https://mxtoolbox.com/blacklists.aspx)
- [ ] GoPhish campaign configuration
                - Create email template - Make sure to edit through the HTML section. you can use `{{.url}}` to include a link to your landing page.
                - Create landing page - You can try using the import site feature through gophish but a much better way is using the "save page WE" chrome or firefox extension. This will copy a page to a local .html file. Cat out the file and paste it into gophish. use this [link](https://docs.getgophish.com/user-guide/faq) to troubleshoot form submission not capturing data. 
                - set up sending profile - host field should be `localhost:25`. Add a custom header with the header "X-Mailer" and the value to a single whitespace. This will remove the X-Mailer: gophish" header.
                - Set up user and group - you can bulk upload by having a .csv file in Firstname,lastname,email,title format. I would also create a test group.
                - Set up campaign - url field should be `https://yourdomain/`. WARNING: if you do not set a launch date and you submit this section it will start sending out emails.
                - To create a static webpage, `mkdir /opt/gophish/static/endpoint` then `echo "test" > index.html`, and then visit https://HOSTNAME/static/index.html 
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
