---
title: '[CSAW 2016] Sleeping Guard Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:02:43+00:00
url: /csaw-2016-sleeping-guard-writeup/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - Crypto
  - CSAW-2016
  - CTF
  - Writeups

---
**Description:**

> _Only true hackers can see the image in this magic PNG&#8230;._  
>  _nc crypto.chal.csaw.io 8000_
> 
> _Author: Sophia D&#8217;Antoine_  
> _<a class="chal-file" href="https://ctf.csaw.io/static/uploads/69a76e75bc0277cb8ead3782870dee13/sleeping_dist.py" target="_blank">sleeping_dist.py</a>_

We are given with python script, Netcat command and a hint about a PNG file. Let&#8217;s run Netcat and see what we will get:

```vim
[Megabeets] /tmp/CSAW/clam# nc crypto.chal.csaw.io 8000
3j8PL1JLRUFleSEyHicFOl9BXrdleSGXX2lBaF9EZRcjeSE/UwgAJR5BX/rqct1eUm9BaH8iFxkoeSFFcW9B6NtBX7FleSG/v29BHW9BX6EFeSEFz29Bfy/d5RpZeSE/Xh8JMSxBX1kReSEtI26fDkA5X0tkIEhrDxsZJRN7PCQIV0BbOA0kRicsL0tleSE/axd7EDIxMi4RGAFHOgMvG2U5YmkEHU5
...
&lt;alot of base64 text here&gt;
...
```


We received a base64 encoded text from the server. It is probably our image so let&#8217;s decode it and save it to file:

```vim
[Megabeets]$&gt; nc crypto.chal.csaw.io 8000 | base64 --decode &gt; out.png
```


Trying to open the image we faced with an error, our image-viewer could not open the file. Open the file with text viewer and see that there is no PNG header. So, we have the image but it somehow encoded and we need to find out how to decode it. Let&#8217;s look at the script, the answer will probably be there. It&#8217;s not so long so I attached it here:

```python
import base64
from twisted.internet import reactor, protocol
import os

PORT = 9013

import struct
def get_bytes_from_file(filename):  
    return open(filename, "rb").read()  
    
KEY = "[CENSORED]"

def length_encryption_key():
    return len(KEY)

def get_magic_png():
    image = get_bytes_from_file("./sleeping.png")
    encoded_string = base64.b64encode(image)
    key_len = length_encryption_key()
    print 'Sending magic....'
    if key_len != 12:
        return ''
    return encoded_string 
    

class MyServer(protocol.Protocol):
    def connectionMade(self):
        resp = get_magic_png()
        self.transport.write(resp)

class MyServerFactory(protocol.Factory):
    protocol = MyServer

factory = MyServerFactory()
reactor.listenTCP(PORT, factory)
reactor.run()
```


Look at the highlighted rows. You can see that after encoding the file with base64 the script is checking whether the size of the encryption key is 12 . We don&#8217;t see any encryption in the script except the encoding itself but we can assume that in the original script an encryption is done using 12 bytes long key. But what encryption? There are billion of options, how can we find the right decryption algorithm to use? Well, the answer is simple &#8211; this is a CTF and the admins know that we cannot try all the possible decryption methods so it will probably be the banal option: XOR.

After choosing our encryption method let&#8217;s think how can we find the key itself. We know the file is a PNG image, so we can XOR the first 12 bytes of the encrypted flle with the first 12 bytes of normal PNG file.

<span style="font-size: 10pt;">89 50 4E 47 0D 0A 1A 0A 00 00 00 0D</span> **XOR** <span style="font-size: 10pt;">DE 3F 0F 2F 52 4B 45 41 65 79 21 32</span> **==** <span style="font-size: 10pt;">57 6F 41 68 5F 41 5F 4B 65 79 21 3F  </span>

<span style="font-size: 12pt;">which  in ASCII is: &#8220;<em>WoAh_A_Key!?&#8221;</em></span>

Now that we have the key we can let python do it&#8217;s magic:

```python
def xor(data, key):
    l = len(key)
    return bytearray((
        (data[i] ^ key[i % l]) for i in range(0,len(data))
    ))

# Read the encrypted image as bytearray
data = bytearray(open('out.png', 'rb').read())

# This is our key as bytearray: "WoAh_A_Key!?"
key = bytearray([0x57, 0x6f, 0x41, 0x68, 0x5f, 0x41, 0x5f, 0x4b, 0x65, 0x79, 0x21, 0x3f])

with open('decrypted.png', 'w') as file_:
    file_.write(xor(data,key))

```


And you&#8217;ll get the image and the flag:

<img src="../uploads/sleping_guard.png" />  


<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>