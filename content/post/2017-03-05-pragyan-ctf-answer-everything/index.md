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

```diff
Megabeets$ file ./main.exe
main.exe: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=4b9b47b7eac612e0c367f0e3a9878eb1f09b841d, not stripped
root:/mnt/d/
```


&nbsp;

Haha, weird. It is actually an ELF file and not exe. Lets execute the binary and give it the answer to everything (&#8217;42&#8217;) as an input:

```diff
Megabeets$ ./main.exe
Gimme: 42
Cipher from Bill
Submit without any tags
#kdudpeh
```


At first I though that &#8220;#kdudpeh&#8221; is the flag but it isn&#8217;t, neither &#8220;kdudpeh&#8221;. The name of the person in the question is Shal, looking like SHA1, and the binary says &#8220;submit without any tags&#8221;, so &#8220;**hashtag**kdudpeh&#8221; without the tag is just &#8220;**hash**kdudpeh&#8221;. So I tried to submit the result of SHA1(&#8220;kdudpeh&#8221;) as answer but failed again. Then I tried Caesar cipher on &#8220;kdudpeh&#8221; and find &#8220;**harambe&#8221;.**

<img src="../uploads/answer_harambe.jpg" /> 

So I again tried submit the flag, this time with Sha1(&#8220;harabe&#8221;).

The flag was **pragyanctf{31a0d851ea10ad886ad4e99ed05892de06998ab9} **which is `SHA1("harambe")`

&nbsp;

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>