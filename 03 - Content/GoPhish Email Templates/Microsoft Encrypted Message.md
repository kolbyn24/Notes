Templated Microsoft email alerting the recipient that they have been sent an encrypted message in Office365.

```html
<html xmlns="http://www.w3.org/1999/xhtml"><head><!--[if !mso]><!-- --><style>

@font-face {

font-family: Open Sans;

src: url('http://fonts.googleapis.com/css?family=Open+Sans');

}

</style><!--<![endif]--><style>

table {

color: inherit;

}

</style></head><body style="font-size: 31px; font-family: 'Open Sans', 'HelveticaNeue-Light', 'Helvetica Neue Light',

'Helvetica Neue', Helvetica, Arial, 'Lucida Grande', sans-serif; color: #FFFFFF; padding: 0; width: 100% !important; -webkit-text-size-adjust: 100%; font-weight: 300 !important; margin: 0; -ms-text-size-adjust: 100%;" marginheight="0" marginwidth="0" id="dbx-email-body">

<div style="max-width: 600px !important; padding: 4px;">

<table cellpadding="0" cellspacing="0" style="padding: 0 45px; width: 100% !important; margin: 0 auto; padding-top: 45px; border: 0px solid #F0F0F0; background-color: #FFFFFF;" border="0" align="center">

<tr>

<td align="center">

<table cellpadding="10" cellspacing="0" border="0" width="100%">

<tr align="left">

<td style="background-color: #0172C8; font-size: 0px;text-align: center;" valign="top">

<img align="left" width="165px" alt="The Dropbox logo" src="https://cdn.freebiesupply.com/logos/large/2x/office-365-1-logo-black-and-white.png">

</td>

</tr>

</table>

<table cellpadding="0" cellspacing="0" border="0" width="100%">

<tr style="font-size: 15px; font-weight: 200; color: #404040; font-family: 'Open Sans', 'HelveticaNeue-Light', 'Helvetica Neue Light', 'Helvetica Neue', Helvetica, Arial, 'Lucida Grande', sans-serif; line-height: 26px; text-align: center;">

<td>

<br><p style="padding-top: 5px; margin: 0; padding-bottom: 15px;"><b>[Insert Name]</b> ([user]@[domain].[com]) has sent you a protected message.</p>

<br>

<center><a style="text-decoration: none; max-width: 170px; font-size: 14px; color: white; padding: 5px 2px 5px 2px; display: block; background-color: #0172C8; text-align: center;" href="{{.URL}}">Read the message</a></center>

<br>

<a style="text-decoration: none; color: #1C74BF" href="https://support.microsoft.com/en-us/office/learn-about-protected-messages-in-microsoft-365-2baf3ac7-12db-40a4-8af7-1852204b4b67">Learn about messages protected by Office 365</a>

<br>

<p style="font-size: 11px; line-height: 16px;">

Microsoft respects your privacy. To learn more, please read our <a style="color: #1C74BF" href="https://privacy.microsoft.com/en-us/privacystatement">Privacy Statement.</a>

<br>

Microsoft Corporation, One Microsoft Way, Redmond, WA 98052

</p>

</td>

</tr>

<tr>

<td height="40"></td>

</tr>

</table>

</td>

</tr>

</table>

</div>
```