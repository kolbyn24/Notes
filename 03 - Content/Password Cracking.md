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

# [[Password Cracking]]  
___

## Description:  

# John The Ripper

```
sudo unshadow ./passwd ./shadow > unshadow

sudo john --wordlist=/usr/share/wordlists/rockyou.txt unshadow

sudo john --show unshadow
```

### Generate a dictionary from HTML pages
```
root@kali:~# html2dic

Uso: ./html2dic <file>
```

### Custom dictionaries
```
root@kali:~# gendict

Usage: gendict -type pattern

  type: -n numeric [0-9]

        -c character [a-z]

        -C uppercase character [A-Z]

        -h hexa [0-f]

        -a alfanumeric [0-9a-z]

        -s case sensitive alfanumeric [0-9a-zA-Z]

  pattern: Must be an ascii string in which every 'X' character wildcard

           will be replaced with the incremental value.

Example: gendict -n thisword_X

  thisword_0

  thisword_1

  [...]

  thisword_9
```

# hashcat
 [hashcat](https://hashcat.net/hashcat/)

To crack password hashes, we need to transform them into the expected format. The [example hashes page](https://hashcat.net/wiki/doku.php?id=example_hashes) can help.

### Cracking Kerberoasting Hashes
Use `--format=krb5tgs --wordlist=wordlist svc_mssql` for **john** or `-a 0 -m 13100 svc_mssql wordlist` for **hashcat**.

```
root@kali:~# john --format=krb5tgs --wordlist=wordlist svc_mssql
Cyberb0tic       (svc_mssql$dev.cyberbotic.io)
```


### Cracking AS-REP Roasting Hashes
Use `--format=krb5asrep --wordlist=wordlist svc_oracle` for **john** or `-a 0 -m 18200 svc_oracle wordlist` for **hashcat**.

```
root@kali:~# john --format=krb5asrep --wordlist=wordlist svc_oracle
Passw0rd!        ($krb5asrep$svc_oracle@dev.cyberbotic.io)
```

### Cracking NTLMv2 Hashes

Use `--format=netntlmv2 --wordlist=wordlist svc_mssql-netntlmv2` with **john** or `-a 0 -m 5600 svc_mssql-netntlmv2 wordlist` with **hashcat** to crack.

### Wordlist

A "wordlist" or "dictionary" attack is the easiest mode of password cracking, in which we simply read in a list of password candidates and try each one line-by-line. There are many popular lists out there, including the venerable rockyou list. The [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords) repo also have an expansive collection for different applications.

 Use `hashcat.exe --help` to get a complete list of attack mode and hash types.


```
D:\Tools\hashcat-6.1.1>hashcat.exe -a 0 -m 1000 C:\Temp\ntlm.txt D:\Tools\rockyou.txt
hashcat (v6.1.1) starting...

58a478135a93ac3bf058a5ea0e8fdb71:Password123
```

-   `-a 0` specifies the wordlist attack mode.
-   `-m 1000` specifies that the hash is NTLM.
-   `C:\Temp\ntlm.txt` is a text file containing the NTLM hash to crack.
-   `D:\Tools\rockyou.txt` is the wordlist.

### Wordlist + Rules

Rules are a means of extending or manipulating the "base" words in a wordlist in ways that are common habits for users. Such manipulation can include toggling character cases (e.g. `a` to `A`), character replacement (e.g. `a` to `@`) and prepending/appending characters (e.g. `password` to `password!`).

The [hashcat wiki](https://hashcat.net/wiki/doku.php?id=rule_based_attack) contains all the information we need to write a custom rule that will append the year 2020 to each word in rockyou. Hashcat also ships with lots of rule files in the rules directory that you can use.

```
D:\Tools\hashcat-6.1.1>hashcat.exe -a 0 -m 1000 C:\Temp\ntlm.txt D:\Tools\rockyou.txt -r rules\add-year.rule
hashcat (v6.1.1) starting...

acbfc03df96e93cf7294a01a6abbda33:Summer2020
```

-   `-r rules\add-year.rule` is our custom rule file

### Masks

A mask attack is an evolution over the brute-force and allows us to be more selective over the keyspace in certain positions.
For instance, instead of using the full keyspace on the first character, we can limit ourselves to uppercase only and likewise with the other positions.

```
D:\Tools\hashcat-6.1.1>hashcat.exe -a 3 -m 1000 C:\Temp\ntlm.txt ?u?l?l?l?l?l?l?l?d
hashcat (v6.1.1) starting...

64f12cddaa88057e06a81b54e73b949b:Password1
```
-   `-a 3` specifies the mask attack.
-   `?u?l?l?l?l?l?l?l?d` is the mask.

`hashcat --help` will show the charsets and are as follows:


```
? | Charset
===+=========
l | abcdefghijklmnopqrstuvwxyz
u | ABCDEFGHIJKLMNOPQRSTUVWXYZ
d | 0123456789
h | 0123456789abcdef
H | 0123456789ABCDEF
s | !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
a | ?l?u?d?s
b | 0x00 - 0xff
```


By default, this mask attack sets a static password length - `?u?l?l?l?l?l?l?l?1` defines 9 characters, which means we can only crack a 9 character password. To crack passwords of different lengths, we have to manually adjust the mask accordingly.

Hashcat mask files make this process a lot easier for custom masks that you use often.

```
PS C:\> cat D:\Tools\example.hcmask
?d?s,?u?l?l?l?l?1
?d?s,?u?l?l?l?l?l?1
?d?s,?u?l?l?l?l?l?l?1
?d?s,?u?l?l?l?l?l?l?l?1
?d?s,?u?l?l?l?l?l?l?l?l?1
```


```
D:\Tools\hashcat-6.1.1>hashcat.exe -a 3 -m 1000 C:\Temp\ntlm.txt D:\Tools\example.hcmask
hashcat (v6.1.1) starting...

Status...........: Exhausted
Guess.Mask.......: ?u?l?l?l?l?1 [6]

[...snip...]

Guess.Mask.......: ?u?l?l?l?l?l?1 [7]

820be3700dfcfc49e6eb6ef88d765d01:Chimney!
```

Masks can even have static strings defined, such as a company name or other keyword you suspect are being used in passwords.
```
PS C:\> cat D:\Tools\example2.hcmask
ZeroPointSecurity?d
ZeroPointSecurity?d?d
ZeroPointSecurity?d?d?d
ZeroPointSecurity?d?d?d?d
```

```
D:\Tools\hashcat-6.1.1>hashcat.exe -a 3 -m 1000 C:\Temp\ntlm.txt D:\Tools\example2.hcmask
hashcat (v6.1.1) starting...

f63ebb17e157149b6dfde5d0cc32803c:ZeroPointSecurity1234
```

### kwprocessor

 [kwprocessor](https://github.com/hashcat/kwprocessor)

This is a utility for generating key-walk passwords, which are based on adjacent keys such as `qwerty`, `1q2w3e4r`, `6yHnMjU7` and so on.

There are several examples provided in the **basechars**, **keymaps** and **routes** directory in the kwprocessor download.

```
D:\Tools\kwprocessor>kwp64.exe basechars\custom.base keymaps\uk.keymap routes\2-to-10-max-3-direction-changes.route -o D:\Tools\keywalk.txt
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: September 23rd 2022 (08:34 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
