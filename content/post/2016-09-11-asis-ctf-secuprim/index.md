---
title: '[ASIS CTF] SecuPrim Writeup'
author: Megabeets
type: post
date: 2016-09-11T17:02:35+00:00
url: /asis-ctf-secuprim/
categories:
  - ASIS CTF 2016
  - Writeups
tags:
  - ASIS-CTF
  - CTF
  - PPC

---
> _**Description:**_  
>  _Test your might._  
> _secuprim.asis-ctf.ir 42738_

Who doesn&#8217;t love a good PPC challenge? We provided with only a URL and Port so I ran Netcat and faced a bot detection system asking me for &#8216;X&#8217;. The message said that |X|=4. I gave the 2 possible options for absolute value of 4 and those were wrong answers.

```default
[Megabeets]$ nc secuprim.asis-ctf.ir 42738
Bot detection: Are you ready?
ASIS needs proof of work to start the Math challenge.
SHA256(X + "YNT7TFm4gVadh44qNzwQdG").hexdigest() = "9a1f5add2c9198721d5efe3ba4512866...",
X is a string of alphanumeric and |X| = 4
Enter X: 4
Sorry, Bad proof of work!

[Megabeets]$ nc secuprim.asis-ctf.ir 42738
Bot detection: Are you ready?
ASIS needs proof of work to start the Math challenge.
SHA256(X + "tu1uQei0DpFfmmKaF1rdAH").hexdigest() = "1b4d598ef4e9e86dc1adb7d862e7b35f...",
X is a string of alphanumeric and |X| = 4
Enter X: -4
Sorry, Bad proof of work!
```


Well, if |X| isn&#8217;t for &#8216;absolute value of()&#8217; then it must be &#8216;length of()&#8217;. You can notice that both the string appended to X and the SHA256 result are changing in every connection. I wrote a python code to calculate the answer. You can find it in the script embedded below.  After answering I got another test which I&#8217;ve been asked to solve 30 times (with a different value each time):

```default
Good work, let's Go!

In each stage tell us the number of primes or perfect power integers in given range
-----------------------------------------------------------------------------------
What's the number of primes or perfect powers like n such that: 938663777872425905508901094461658229700971384281663171048305722544018188212593585457097324115543346387856004047801971862171751790325297281452399266743172190627763744903214644942745803882444165938580204577049548534754135264523 &lt;= n &lt;= 938663777872425905508901094461658229700971384281663171048305722544018188212593585457097324115543346387856004047801971862171751790325297281452399266743172190627763744903214644942745803882444165938580204577049548534754135266078
```


I wrote the following script and got the flag:



<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>