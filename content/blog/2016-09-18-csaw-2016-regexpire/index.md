---
title: '[CSAW 2016] Regexpire Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:01:11+00:00
url: /csaw-2016-regexpire/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - PPC
  - Writeups

---
**Description:**

> _I thought I found a perfect match but she ended up being my regEx girlfriend._
> 
> _nc misc.chal.csaw.io 8001_

It wasn&#8217;t so hard, I asked google for the best way to generate matched string to a given pattern and wrote the following script. The only headache was when my generator used newlines (&#8220;\n&#8221;) so I removed them.

```python
from pwn import *
import rstr
import exrex
from time import sleep
import re

# conect to server
r = remote('misc.chal.csaw.io', 8001)

# Print the question string
print r.recvline()

# Counter
i=1

while True:
	# Recieve the regex pattern
    reg = r.recvline()[:-1]
    print "%d -------\n"%i
    print reg
    print "-------\n"
    ans=rstr.xeger(reg).replace('\n','') # Remove newlines!
    # ans=exrex.getone(reg).replace('\n','')  # Another possible option
    r.sendline(ans)
    i+=1
	sleep(0.2)
```


And after 1000 tests we got the flag: _flag{^regularly\_express\_<wbr />yourself$}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="./megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>