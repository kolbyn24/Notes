---
creation date: March 29th 2022
last modified date: March 29th 2022
aliases: []
tags: #🐟
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[Phishing Templates]]
Search Tag: #🐟

# [[SpeedTest]]  
___

## Variables 
{{ CLIENT_NAME }} - e.g. Acme 
{{ CLIENT_IT_TEAM }} - e.g. Acme Technology Department
{{ PRETEXT }} - This text will be shown in the e-mail preview 
{{ LOGO_URL }} - A url to the client's logo 
{{ DEADLINE_DATE }} - e.g. February 4, 2022

```html
<!doctype html>
<html>
<head><meta name="viewport" content="width=device-width, initial-scale=1.0"/><meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	<title>{{ CLIENT_NAME }} Speed Test</title>
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
          border: solid 1px #162e51;
          border-radius: 5px;
          box-sizing: border-box;
          color: #da532c;
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
        background-color: #162e51;
      }

      .btn-primary a {
        background-color: #162e51;
        border-color: #162e51;
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
<p><span class="preheader">{{ PRETEXT }}</span></p>

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
									<p><img src="{{ LOGO_URL }}"/></p>

									<p>&nbsp;</p>
									</td>
								</tr>
								<tr>
									<td>
									<p>{{.FirstName}},</p>

									<table border="0" cellpadding="0" cellspacing="0" class="btn btn-primary" role="presentation">
										<tbody>
											<tr>
												<td align="left">{{ CLIENT_NAME }} {{ CLIENT_IT_TEAM }} is entrusted with managing and safeguarding enterprise mission critical infrastructure and information technology systems. As technology is constantly evolving, {{ CLIENT_IT_TEAM }} staff continuously explore and invest in new technology innovations, system modernization, and business solutions that will contribute to the success of {{ CLIENT_NAME }}.<br />
												<br />
												{{ CLIENT_IT_TEAM }} is utilizing a third-party to assist in increasing data speeds and updating wireless access points across {{ CLIENT_NAME }} office locations. In order to get the best possible performance, {{ CLIENT_IT_TEAM }} staff is requiring all {{ CLIENT_NAME }} employees to run an internet speed test from their workstations to ensure accurate data modeling. All {{ CLIENT_NAME }} employees currently working remotely should run this speed test while connected to the {{ CLIENT_NAME }} VPN where applicable.<br />
												<br />
												Please take a moment to run the speed test below. All speed test results are due by 10:00AM on Friday {{ DEADLINE_DATE }}.<br />
												&nbsp;
												<p>&nbsp;</p>

												<table border="0" cellpadding="0" cellspacing="0" role="presentation">
													<tbody>
													</tbody>
													<tbody>
														<tr>
															<td align="left">
															<table border="0" cellpadding="0" cellspacing="0" role="presentation">
																<tbody>
																	<tr>
																		<td><a href="{{.URL}}" target="_blank">Run A Speed Test</a></td>
																	</tr>
																</tbody>
															</table>
															</td>
														</tr>
													</tbody>
												</table>
												</td>
											</tr>
										</tbody>
									</table>
									</td>
								</tr>
								<!-- END MAIN CONTENT AREA -->
							</tbody>
						</table>
						<!-- END CENTERED WHITE CONTAINER --></td>
					</tr>
				</tbody>
			</table>
			</div>
			</td>
		</tr>
	</tbody>
</table>
</body>
</html>
```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: March 29th 2022 (01:31 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
