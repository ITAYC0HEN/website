---
title: '[Pragyan CTF] The Vault'
author: Megabeets
type: post
date: 2017-03-05T07:32:17+00:00
url: /pragyan-ctf-vault/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - CTF
  - Misc
  - Writeups

---
## Description:

> <div class="challenge-description">
>   [!@# a-z $%^ A-Z &* 0-9] [1,3]
> </div>
> 
> <div class="challenge-files">
>   <div>
>     <span class="challenge-attachment"><a class="has-tooltip" title="" href="https://ctf.pragyan.org/download?file_key=e6ddbdba43b6d7d9261769def938d922071984306d03af07005853c26d0739a4&team_key=a500afc4a171f394f280518fefd78d62f976bf8303f77f3431573fce01c983cb" data-toggle="tooltip" data-placement="right" data-original-title="1.15 KB">file</a></span>
>   </div>
> </div>

<div>
</div>

<div>
</div>

<div>
  All we got is a file and regular expression.
</div>

<div>
  Lets run <code>file</code> command on the file to determine its type:
</div>

<div>
  <pre class="lang:diff decode:true ">$ file ./file.kdb
file: Keepass password database 1.x KDB, 3 groups, 4 entries, 50000 key transformation rounds</pre>
  
  <p>
    The file is KDB file which is Keepass password database. Keepass is a famous opensource password manager.
  </p>
  
  <p>
    I tried open it using KeePassX for windows, but we need a password to open the database. The password probably should match the regex, so I generated a dictionary with all the possible passwords (more then 300,000 words).
  </p>
  
  <pre class="lang:python decode:true ">import string
import itertools

# strings match the regex
chars = string.lowercase + string.uppercase + string.digits + '!@#$%^&*'
f = open('dict.txt','a')

all_permutations = list(itertools.permutations(chars,1))+ list(itertools.permutations(chars,2))+ list(itertools.permutations(chars,3))

for p in all_permutations:
    f.write(''.join(p)+'\n')</pre>
  
  <p>
    &nbsp;
  </p>
</div>

And I the ran John the Ripper to crack the password and went to eat lunch.

<pre class="toolbar:2 show-lang:2 nums:false nums-toggle:false lang:diff decode:true ">$ keepass2john file.kdb &gt; kp
$ john  --wordlist=dict.txt -format:keepass kp
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64 OpenSSL])
Press 'q' or Ctrl-C to abort, almost any other key for status
k18              (file.kdb)
</pre>

When I came back I saw that John found the password, now lets open the file:

<img data-attachment-id="990" data-permalink="https://www.megabeets.net/pragyan-ctf-vault/vault1/#main" data-orig-file="http://www.megabeets.net/uploads/vault1.png" data-orig-size="802,632" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="pragyan_vault1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/vault1-300x236.png" data-large-file="http://www.megabeets.net/uploads/vault1.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-990" src="https://www.megabeets.net/uploads/vault1.png" alt="" width="802" height="632" srcset="https://www.megabeets.net/uploads/vault1.png 802w, https://www.megabeets.net/uploads/vault1-150x118.png 150w, https://www.megabeets.net/uploads/vault1-300x236.png 300w, https://www.megabeets.net/uploads/vault1-768x605.png 768w, https://www.megabeets.net/uploads/vault1-800x630.png 800w" sizes="(max-width: 802px) 100vw, 802px" /> 

&nbsp;

The flag was **pragyanctf{closed\_no\_more}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>