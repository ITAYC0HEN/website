---
title: '[Pragyan CTF] Answer To Everything'
author: Megabeets
type: post
date: 2017-03-05T07:30:52+00:00
url: /pragyan-ctf-answer-everything/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - CTF
  - Writeups

---
## Description:

> Shal has got a binary. It contains the name of a wise man and his flag. He is unable to solve it.
> 
> Submit the flag to unlock the secrets of the universe.
> 
> <span class="challenge-attachment"><a class="has-tooltip" title="" href="https://ctf.pragyan.org/download?file_key=b4e11f9e9abf06eaff141e61d46e57668c1c47d0e4f0db05072de131a07c0af2&team_key=a500afc4a171f394f280518fefd78d62f976bf8303f77f3431573fce01c983cb" data-toggle="tooltip" data-placement="right" data-original-title="8.52 KB">main.exe</a></span>

In this challenge we have a binary, I ran `file` command on it:

<pre class="lang:diff decode:true ">Megabeets$ file ./main.exe
main.exe: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=4b9b47b7eac612e0c367f0e3a9878eb1f09b841d, not stripped
root:/mnt/d/</pre>

&nbsp;

Haha, weird. It is actually an ELF file and not exe. Lets execute the binary and give it the answer to everything (&#8217;42&#8217;) as an input:

<pre class="toolbar:2 toolbar-hide:false show-title:false show-lang:2 nums:false nums-toggle:false lang:diff decode:true">Megabeets$ ./main.exe
Gimme: 42
Cipher from Bill
Submit without any tags
#kdudpeh</pre>

At first I though that &#8220;#kdudpeh&#8221; is the flag but it isn&#8217;t, neither &#8220;kdudpeh&#8221;. The name of the person in the question is Shal, looking like SHA1, and the binary says &#8220;submit without any tags&#8221;, so &#8220;**hashtag**kdudpeh&#8221; without the tag is just &#8220;**hash**kdudpeh&#8221;. So I tried to submit the result of SHA1(&#8220;kdudpeh&#8221;) as answer but failed again. Then I tried Caesar cipher on &#8220;kdudpeh&#8221; and find &#8220;**harambe&#8221;.**

<img data-attachment-id="982" data-permalink="https://www.megabeets.net/pragyan-ctf-answer-everything/answer_harambe/#main" data-orig-file="http://www.megabeets.net/uploads/answer_harambe.jpg" data-orig-size="783,983" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="answer_harambe" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/answer_harambe-239x300.jpg" data-large-file="http://www.megabeets.net/uploads/answer_harambe.jpg" decoding="async" loading="lazy" class="aligncenter size-full wp-image-982" src="https://www.megabeets.net/uploads/answer_harambe.jpg" alt="" width="783" height="983" srcset="https://www.megabeets.net/uploads/answer_harambe.jpg 783w, https://www.megabeets.net/uploads/answer_harambe-119x150.jpg 119w, https://www.megabeets.net/uploads/answer_harambe-239x300.jpg 239w, https://www.megabeets.net/uploads/answer_harambe-768x964.jpg 768w" sizes="(max-width: 783px) 100vw, 783px" /> 

So I again tried submit the flag, this time with Sha1(&#8220;harabe&#8221;).

The flag was **pragyanctf{31a0d851ea10ad886ad4e99ed05892de06998ab9} **which is `SHA1("harambe")`

&nbsp;

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>