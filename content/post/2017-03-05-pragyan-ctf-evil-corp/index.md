---
title: '[Pragyan CTF] Evil Corp'
author: Megabeets
type: post
date: 2017-03-05T07:31:41+00:00
url: /pragyan-ctf-evil-corp/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - Crypto
  - CTF
  - Writeups

---
## Description:

> fsociety has launched another attack at Evil Corp. However, Evil Corp has decided to encrypt the .dat file with a CBC cipher. Reports reveal that it is not AES and the key is relatively simple, but the IV might be long. And remember, fsociety and evilcorp are closely linked.
> 
> **Hint!** Snakes serve the fsociety. Hmmm.
> 
> **Hint!** fsociety and evilcorp are too close, even 16 characters long together. Damn
> 
> <span class="challenge-attachment"><a class="has-tooltip" title="" href="https://ctf.pragyan.org/download?file_key=9939cbc839f8a6439780e2c2eef012464762a396a860469e87f675e1502d0fe5&team_key=a500afc4a171f394f280518fefd78d62f976bf8303f77f3431573fce01c983cb" data-toggle="tooltip" data-placement="right" data-original-title="17.72 KB">fsociety_new.dat</a> </span>

This challenge was tricky for lot of people, the riddle was hiding in the questions itself. The challenge doesn&#8217;t require high skills, just understanding the meaning behind the words and hints.

From the question we know it&#8217;s a CBC cipher, but which? I got it just after the first hint was released, something to do with **snakes**. hmm&#8230; Serpent! Serpent is another term for Snake, and there&#8217;s Serpent-CBC cipher.

<img src="../uploads/serpent1.jpg" /> 

What about the IV? We know several things about the IV:

  1. The length of Serpent-CBC IV must be 32 bytes,  
    2. Most of the Serpent decrypters are taking the IV as hex sequence  
    3. in the question: &#8220;but the IV might be long&#8221;  
    4. in the Hint: &#8220;even 16 characters long together&#8230;fsociety and evilcorp are closely linked&#8221;.

So, this made me believe that the IV is &#8220;fsocietyevilcorp&#8221; because \`len(hex(&#8220;fsocietyevilcorp&#8221;))==32\`.

So we now know the algorithm and the IV, what is the Key? The question says &#8220;the key is relatively simple&#8221;. So I tried [online][1] with some simple and &#8220;obvious&#8221; keys until I recognize a valid header of file and found that the key was &#8220;_fsociety_&#8220;.

<img src="../uploads/evil_1.png" /> 

* * *

We got a leet JPEG image with the flag:

<img src="../uploads/odt-IV-66736f63696574796576696c636f7270.jpg" /> 

* * *

The flag was **pragyanctf{hellofriend}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: http://serpent.online-domain-tools.com/