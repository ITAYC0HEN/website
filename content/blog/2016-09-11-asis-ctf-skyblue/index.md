---
title: '[ASIS CTF] Sky Blue Writeup'
author: Megabeets
type: posts
date: 2016-09-11T17:01:39+00:00

categories:
  - ASIS CTF 2016
  - Writeups
tags:
  - ASIS-CTF
  - CTF
  - Forensics

---
> _**Description**_  
>  _Why is the [sky blue][1]?_

&nbsp;

We are given a PCAP file containing some Bluetooth traffic. The flag has probably been transmitted between the devices. Let&#8217;s see what files has been sent.

```default
[Megabeets]$: binwalk -e blue.pcap

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
40535         0x9E57          PNG image, 1400 x 74, 8-bit colormap, non-interlaced
```


Binwalk found a PNG image but couldn&#8217;t export it. I opened Wireshark and searched for the string &#8220;PNG&#8221; in the packet bytes. I found the 7 packets containing the PNG and exported their packet bytes (i.e Only the **DATA**, without the header bytes of each packet: 02 0C 20 FC 03 F8 03 47 00 63 EF E6 07). I then concatenated the output files using HxD,

<img src="./hxd.png" /> 

and deleted the extra data preceding the PNG file header.

<img src="./hxd2.png" /> 

We now have the PNG file which is the flag:

<img src="./out4.png" /> 



 [1]: http://asis-ctf.ir/tasks/blue.txz_3a987ff102f69adcdad1ced41f9fdff2cba1a7e9