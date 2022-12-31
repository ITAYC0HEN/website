---
title: '[ASIS CTF] Sky Blue Writeup'
author: Megabeets
type: post
date: 2016-09-11T17:01:39+00:00
url: /asis-ctf-skyblue/
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

<pre class="theme:plain-white lang:default decode:true ">[Megabeets]$: binwalk -e blue.pcap

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
40535         0x9E57          PNG image, 1400 x 74, 8-bit colormap, non-interlaced</pre>

Binwalk found a PNG image but couldn&#8217;t export it. I opened Wireshark and searched for the string &#8220;PNG&#8221; in the packet bytes. I found the 7 packets containing the PNG and exported their packet bytes (i.e Only the **DATA**, without the header bytes of each packet: 02 0C 20 FC 03 F8 03 47 00 63 EF E6 07). I then concatenated the output files using HxD,

<img data-attachment-id="316" data-permalink="https://www.megabeets.net/asis-ctf-skyblue/hxd/#main" data-orig-file="http://www.megabeets.net/uploads/hxd.png" data-orig-size="331,351" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="hxd" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/hxd-283x300.png" data-large-file="http://www.megabeets.net/uploads/hxd.png" decoding="async" loading="lazy" class="size-full wp-image-316 aligncenter" src="http://www.megabeets.net/uploads/hxd.png" alt="hxd" width="331" height="351" srcset="https://www.megabeets.net/uploads/hxd.png 331w, https://www.megabeets.net/uploads/hxd-141x150.png 141w, https://www.megabeets.net/uploads/hxd-283x300.png 283w" sizes="(max-width: 331px) 100vw, 331px" /> 

and deleted the extra data preceding the PNG file header.

<img data-attachment-id="318" data-permalink="https://www.megabeets.net/asis-ctf-skyblue/hxd2/#main" data-orig-file="http://www.megabeets.net/uploads/hxd2.png" data-orig-size="613,101" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="hxd2" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/hxd2-300x49.png" data-large-file="http://www.megabeets.net/uploads/hxd2.png" decoding="async" loading="lazy" class="size-full wp-image-318 aligncenter" src="http://www.megabeets.net/uploads/hxd2.png" alt="hxd2" width="613" height="101" srcset="https://www.megabeets.net/uploads/hxd2.png 613w, https://www.megabeets.net/uploads/hxd2-150x25.png 150w, https://www.megabeets.net/uploads/hxd2-300x49.png 300w" sizes="(max-width: 613px) 100vw, 613px" /> 

We now have the PNG file which is the flag:

<img data-attachment-id="319" data-permalink="https://www.megabeets.net/asis-ctf-skyblue/out4/#main" data-orig-file="http://www.megabeets.net/uploads/out4.png" data-orig-size="1400,74" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="out4" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/out4-300x16.png" data-large-file="http://www.megabeets.net/uploads/out4-1024x54.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-319" src="http://www.megabeets.net/uploads/out4.png" alt="out4" width="1400" height="74" srcset="https://www.megabeets.net/uploads/out4.png 1400w, https://www.megabeets.net/uploads/out4-150x8.png 150w, https://www.megabeets.net/uploads/out4-300x16.png 300w, https://www.megabeets.net/uploads/out4-768x41.png 768w, https://www.megabeets.net/uploads/out4-1024x54.png 1024w, https://www.megabeets.net/uploads/out4-800x42.png 800w" sizes="(max-width: 1400px) 100vw, 1400px" /> 

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: http://asis-ctf.ir/tasks/blue.txz_3a987ff102f69adcdad1ced41f9fdff2cba1a7e9