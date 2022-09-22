From the PTP virtual machine console:
```bash
rsync -avz /home/asmtuser/PTP/pentestportal/media/screenshots/ asmtuser@<SHARE IP>:'/share/Working/user/Finding\ Screenshots'
```
(Replace <SHARE IP>)

This will copy all screenshots already uploaded to PTP (in all findings and the technical overview section including phishing assessment, penetration test, and web application assessment) to a folder on the share. This is useful if your fed lead uses the "Readme" HTML for helping the customer navigate their data and/or wants to include copies of the screenshots.

If you add more screenshots to PTP, run rsync again to update the share.

If you delete screenshots from PTP, run rsync again and include the "--delete" switch to delete files from the receiving side (the share) that don't exist in PTP anymore . 

Use "--dry-run" to see the changes that will occur before doing anything.