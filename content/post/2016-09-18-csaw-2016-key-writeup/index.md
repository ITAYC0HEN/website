---
title: '[CSAW 2016] Key Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:02:54+00:00
url: /csaw-2016-key-writeup/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - Reverse
  - Writeups

---
#### **Description:**

> _So I like to make my life difficult, and instead of a password manager, I make challenges that keep my secrets hidden. I forgot how to solve this one and it is the key to my house&#8230; Can you help me out? It&#8217;s getting a little cold out here._
> 
> _NOTE: Flag is not in normal flag format._
> 
> <div class="chal-files">
>   <em><a class="chal-file" href="https://ctf.csaw.io/static/uploads/6c3bc1cb1618348f549dd059ed2bf23d/key.exe" target="_blank">key.exe</a></em>
> </div>

<div class="chal-files">
</div>

<div class="chal-files">
  Running the file we end up with a message: &#8220;?W?h?a?t h?a?p?p?e?n?&#8221;
</div>

<div class="chal-files">
  Let&#8217;s open the exe in IDA and view it&#8217;s strings looking for interesting strings.
</div>

<div class="chal-files">
</div>

<div class="chal-files">
  <pre class="lang:asm decode:true ">.rdata:00AB52B8 00000029 C C:\\Users\\CSAW2016\\haha\\flag_dir\\flag.txt
.rdata:00AB52E4 00000016 C ?W?h?a?t h?a?p?p?e?n?                        
.rdata:00AB52FC 00000021 C |------------------------------|             
.rdata:00AB5320 00000021 C |==============================|             
.rdata:00AB5344 00000021 C \\  /\\  /\\  /\\  /\\==============|        
.rdata:00AB5368 00000021 C  \\/  \\/  \\/  \\/  \\=============|        
.rdata:00AB538C 00000021 C                  |-------------|             
.rdata:00AB53B0 00000015 C Congrats You got it!                         
.rdata:00AB53C8 00000012 C =W=r=o=n=g=K=e=y=</pre>
  
  <p>
    We have 4 interesting strings:
  </p>
  
  <ul>
    <li>
      <strong>A path: </strong>C:\\Users\\CSAW2016\\haha\\flag_dir\\flag.txt
    </li>
    <li>
      <strong>The known message: </strong>?W?h?a?t h?a?p?p?e?n?
    </li>
    <li>
      <strong>Good key: </strong>Congrats You got it!
    </li>
    <li>
      <strong>Bad key:</strong> =W=r=o=n=g=K=e=y=
    </li>
  </ul>
  
  <p>
    Visiting the function that uses the path string (X-ref) we understand the program is trying to read the key from it, if it doesn&#8217;t exists we would get: ?W?h?a?t h?a?p?p?e?n?
  </p>
  
  <p>
    I Created the txt file with &#8220;aaa&#8221; inside and ran again, this time I set a breakpoint before the decision whether to jump to the success or failure message.
  </p>
  
  <p>
    <img src="../uploads/asm_key_csaw.png" />
  </p>
  
  <p>
    Now let&#8217;s see what we have in what seem like the comparison function.
  </p>
  
  <p>
    Stepping the lines we can see that my &#8220;aaa&#8221; is compared with a string.
  </p>
  
  <p>
    <img src="../uploads/csaw_key_eax.png" />
  </p>
  
  <p>
    This string is the key &#8220;<em>idg_cni~bjbfi|gsxb</em>&#8221; and also the flag to the challenge.
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <div class="nf-post-footer">
    <p style="text-align: right">
      <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
    </p>
  </div>