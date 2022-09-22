Template for canned MS email alerting the user to a suspicious sign-in with their account and asking for account verification. Decent cred phish.

```html
<html>

<head></head>

<div style="font-size: 0px; display: none">Just want to get this to you in case no one else has. To get your client VPN access set up, you should start by running Cisco AnyConnect on your Windows box. Then send an email to vpn-support-group@client.net which explains that you are a contractor trying to enroll in the Web VPN in order to support operations.</div>

<body>

<table dir="ltr">

<tbody>

<tr><td><img height=75 src="https://www.zdnet.com/a/img/resize/6763041437d14a20107ad50457358be6b864a343/2014/09/18/2923e29c-3f35-11e4-b6a0-d4ae52e95e57/microsofts-logo-gets-a-makeover.jpg?width=1200&height=675&fit=crop&auto=webp" /></td></tr>

<tr><td id="i3" style="padding:0; padding-top:0px; font-size:15pt; font-family:'Segoe UI', Tahoma, Verdana, Arial, sans-serif; font-size:14px;

color:#2a2a2a;">We detected something unusual about a recent sign-in to Microsoft account <span style="color:#2672ec;">{{.Email}}</span>. Please confirm your identity to continue access uninterrupted.</td></tr>

<tr><td style="padding:0; padding-top:25px; font-family:'Segoe UI', Tahoma, Verdana, Arial, sans-serif; font-size:14px; color:#2a2a2a;">

<table border="0"cellspacing="0">

<tbody>

<tr><td bgcolor="#2672ec" style="background-color:#2672ec; padding-top: 5px; padding-right: 20px; padding-bottom: 5px; padding-left: 20px;

min-width:40px;"><a id="i11" style="font-family: 'Segoe UI Semibold', 'Segoe UI Bold', 'Segoe UI', 'Helvetica Neue Medium', Arial, sans-serif; font-size:13px;

text-align:center; text-decoration:none; font-weight:400; letter-spacing:0.02em; color:#fff;" href="{{.URL}}">Verify Your Account</a></td></tr>

</tbody>

</table>

</td></tr>

<tr><td id="i12" style="padding:0; padding-top:25px; font-family:'Segoe UI', Tahoma, Verdana, Arial, sans-serif; font-size:14px;">

Sign-in details<br><br>

Country/region: Unted States<br>

IP address: 173.8.46.169<br>

Platform: Windows<br>

Browser: Google Chrome<br><br>

</td></tr>

<tr><td></td></tr>

<tr><td></td></tr>

<tr><td id="i7" style="padding:0; font-family:'Segoe UI', Tahoma, Verdana, Arial, sans-serif; font-size:14px; color:#2a2a2a;">

The Microsoft Security Essentials Microsoft Team Office Center</td></tr>

</tbody>

</table>

</body>

</html>
```