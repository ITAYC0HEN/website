---
title: '[Pragyan CTF] Game of Fame'
author: Megabeets
type: posts
date: 2017-03-05T07:30:05+00:00

categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - Crypto
  - CTF
  - Writeups

---
## Description:

> p xasc. a zdmik qtng. yiy uist. easc os iye iq trmkbumk. gwv wolnrg kaqcs vi rlr.
> 
> **Hint!** Robert Sedgewick

To be honest, this challenge was pretty simple. I decrypted the text using online [Vigenere cipher][1] decrypter, which is the first cipher I try in suchcases, just after Caesar cipher.

The key was &#8220;pragyan&#8221; and the result was: _&#8220;a game. a movie star. his wife. **name of the cs textbook**. the winner takes it all.&#8221;_

I then used the hint about Robert Sedgewick, which is a famous computer science professor at Princeton University. I found that the flag is his CS textbook title.

The flag was **pragyanctf{algorithms}**.



 [1]: https://www.guballa.de/vigenere-solver