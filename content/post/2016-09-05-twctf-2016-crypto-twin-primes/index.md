---
title: '[TWCTF-2016: Crypto] Twin Primes Writeup'
author: Megabeets
type: post
date: 2016-09-05T00:00:52+00:00
url: /twctf-2016-crypto-twin-primes/
categories:
  - TWCTF 2016
  - Writeups
tags:
  - Crypto
  - CTF
  - TWCTF
  - Writeups

---
> **Challenge description:**  
> _Decrypt it._  
> [twin-primes.7z][1]{.attachment}

* * *

We have 4 files in the archive:

  * **encrypt.py** &#8211; A Python script uses RSA algorithm to encrypt the flag
  * **encryped** &#8211; The encrypted message
  * **key 1** &#8211; n, and e of one of the keys used in the encryption process
  * **key 2** &#8211; n, and e of the other key used in the encryption process

Are you ready for your math lesson? Here we go.  
After reading encrypt.py we know that:

  * <span id="MathJax-Element-1-Frame" class="MathJax" style="margin: 0px; padding: 0px; border: 0px; font-size: 15px; display: inline; font-style: normal; font-weight: normal; line-height: normal; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; position: relative;" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mtable columnalign=&quot;right left right left right left right left right left right left&quot; rowspacing=&quot;3pt&quot; columnspacing=&quot;0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em&quot; displaystyle=&quot;true&quot;><mtr><mtd><msub><mi>n</mi><mn>1</mn></msub></mtd><mtd><mi></mi><mo>=</mo><mi>p</mi><mi>q</mi></mtd></mtr><mtr><mtd><msub><mi>n</mi><mn>2</mn></msub></mtd><mtd><mi></mi><mo>=</mo><mo stretchy=&quot;false&quot;>(</mo><mi>p</mi><mo>+</mo><mn>2</mn><mo stretchy=&quot;false&quot;>)</mo><mo stretchy=&quot;false&quot;>(</mo><mi>q</mi><mo>+</mo><mn>2</mn><mo stretchy=&quot;false&quot;>)</mo></mtd></mtr></mtable></math>"><span id="MathJax-Span-1" class="math"><span id="MathJax-Span-2" class="mrow"><span id="MathJax-Span-3" class="mtable"><span id="MathJax-Span-4" class="mtd"><span id="MathJax-Span-5" class="mrow"><span id="MathJax-Span-6" class="msubsup"><span id="MathJax-Span-7" class="mi">n1 = p*q</span></span></span></span></span></span></span></span>
  * n2 <span id="MathJax-Span-20" class="mtd"><span id="MathJax-Span-21" class="mrow"><span id="MathJax-Span-23" class="mo">= </span><span id="MathJax-Span-24" class="mo">(</span><span id="MathJax-Span-25" class="mi">p</span><span id="MathJax-Span-26" class="mo">+</span><span id="MathJax-Span-27" class="mn">2</span><span id="MathJax-Span-28" class="mo">)</span><span id="MathJax-Span-29" class="mo">(</span><span id="MathJax-Span-30" class="mi">q</span><span id="MathJax-Span-31" class="mo">+</span><span id="MathJax-Span-32" class="mn">2</span><span id="MathJax-Span-33" class="mo">)</span></span></span>
  * <span id="MathJax-Element-2-Frame" class="MathJax" style="margin: 0px; padding: 0px; border: 0px; font-size: 15px; display: inline; font-style: normal; font-weight: normal; line-height: normal; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; position: relative;" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>p</mi></math>"><span class="MJX_Assistive_MathML">p</span></span> and <span id="MathJax-Element-3-Frame" class="MathJax" style="margin: 0px; padding: 0px; border: 0px; font-size: 15px; display: inline; font-style: normal; font-weight: normal; line-height: normal; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; position: relative;" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>q</mi></math>"><span class="MJX_Assistive_MathML">q</span></span> are <a href="https://en.wikipedia.org/wiki/Twin_prime" target="_blank">twin primes</a>. _i.e _<span id="MathJax-Element-4-Frame" class="MathJax" style="margin: 0px; padding: 0px; border: 0px; font-size: 15px; display: inline; font-style: normal; font-weight: normal; line-height: normal; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; position: relative;" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>p</mi></math>"><span class="MJX_Assistive_MathML">p</span></span> is prime and <span id="MathJax-Element-5-Frame" class="MathJax" style="margin: 0px; padding: 0px; border: 0px; font-size: 15px; display: inline; font-style: normal; font-weight: normal; line-height: normal; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; position: relative;" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>p</mi><mo>+</mo><mn>2</mn></math>"><span id="MathJax-Span-43" class="math"><span id="MathJax-Span-44" class="mrow"><span id="MathJax-Span-45" class="mi">p</span><span id="MathJax-Span-46" class="mo">+</span><span id="MathJax-Span-47" class="mn">2 </span></span></span></span>is also prime; similar for <span id="MathJax-Element-6-Frame" class="MathJax" style="margin: 0px; padding: 0px; border: 0px; font-size: 15px; display: inline; font-style: normal; font-weight: normal; line-height: normal; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; position: relative;" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>q</mi></math>"><span id="MathJax-Span-48" class="math"><span id="MathJax-Span-49" class="mrow"><span id="MathJax-Span-50" class="mi">q.</span></span></span></span>

Now let&#8217;s turn the equation into an equation with one unknown and then solve it for the unknown.We can Isolate q to be <img src="../uploads/twin-primes_1.png" />  and substitute q in the other equation. Now we have an equation in one unknown:<img src="../uploads/twin-primes_2.png" />

Solve the equation and you&#8217;ll get: <img src="../uploads/twin-primes_3.png" />

<div class="MathJax_Display">
  We need to solve this quadratic equation in order to find p and q. After that it will not be a problem to find the d&#8217;s and build the keys.
</div>

<div class="MathJax_Display">
  The rest is in the script:
</div>



<div class="MathJax_Display">
</div>

&nbsp;

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://twctf7qygt6ujk.azureedge.net/uploads/twin-primes.7z-39a1a147cbf55d4d944f8eacdbdf4ee7a967dd70ef0eaaa0a1cee5c58c641483