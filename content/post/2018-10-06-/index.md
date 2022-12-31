---
title: 'Flare-On5 Writeup – Challenge 3: FLEGGO'
author: Megabeets
type: post
date: -001-11-30T00:00:00+00:00
draft: true
url: /?p=1565
categories:
  - Uncategorized

---
> ## FLEGGO {.chal-name.text-center.pt-3}
> 
> <div class="chal-tags text-center">
>
> </div>
> 
> When you are finished with your media interviews and talk show appearances after that crushing victory at the Minesweeper Championship, I have another task for you. Nothing too serious, as you&#8217;ll see, this one is child&#8217;s play.
> 
> 7zip password: infected
> 
> File:
> 
> &nbsp;

&nbsp;

For the 3rd challenge, we are given with a 7z archive file. When extracted we can see dozens of `.exe` files:

/CTF/flare-on/FLEGGO$ ls -1  
1JpPaUMynR9GflWbxfYvZviqiCB59RcI.exe  
2AljFfLleprkThTHuVvg63I7OgjG2LQT.exe  
3Jh0ELkck1MuRvzr8PLIpBNUGlspmGnu.exe  
4ihY3RWK4WYqI4XOXLtAH6XV5lkoIdgv.exe  
7mCysSKfiHJ4WqH2T8ERLE33Wrbp6Mqe.exe  
AEVYfSTJwubrlJKgxV8RAl0AdZJ5vhhy.exe  
BG3IDbHOUt9yHumPceLTVbObBHFneYEu.exe  
Bl0Iv5lT6wkpVCuy7jtcva7qka8WtLYY.exe  
Bp7836noYu71VAWc27sUdfaGwieALfc2.exe  
E36RGTbCE4LDtyLi97l9lSFoR7xVMKGN.exe  
&#8230;  
&#8230;

&nbsp;

Upon opening one of them in [Cutter][1] we can see the following information:

[<img src="../uploads/dashboard-1024x650.png" />][2]

As we can see, this is a rather tiny PE32 file, nothing too special. Let&#8217;s have a look at the `main()` function of the binary.

[<img src="../uploads/fleggo_cutter_main-1024x715.png" />][3]

On the first block, there are two function calls. The first is 0x4012d0 which is responsible for initializing a global array and the second is 0x401050 which is more interesting.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

<pre class=""><code>gci -File *.exe | % { ../strings -u -nobanner $_ | select -last 1 | & $_.fullname}
</code></pre>

&nbsp;

for file in FLEGGO/*; do strings -e l $file | tail -1 | wine $file; done

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://github.com/radareorg/cutter/
 [2]: https://www.megabeets.net/uploads/dashboard.png
 [3]: https://www.megabeets.net/uploads/fleggo_cutter_main.png