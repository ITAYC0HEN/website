---
title: '[H4CK1T 2016] 1n51d3r’5 j0b – Canada Writeup'
author: Megabeets
type: posts
date: 2016-10-02T21:26:53+00:00

categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - Writeups

---
### **Description:**

> **1n51d3r&#8217;5 j0b &#8211; Canada &#8211; 300 &#8211; Forensics**  
> <span style="font-weight: 400;">Tommy wrote a program. It seems he has hidden from us important information. Find out what Tommy hides.</span>  
> [<span style="font-weight: 400;">http://ctf.com.ua/data/attachments/Tommy_2e00c18e3a480959ba5fb4f65ff7f2b7.zip</span>][1]

Oh god, this challenge was so fun. The easiest 300 point I&#8217;ve ever got. I&#8217;m sure it wasn&#8217;t the expected solution but it works so who am I to complain. Three commands, that&#8217;s all.

```sh
Megabeets:/tmp/h4ckit/canada# unzip Tommy_2e00c18e3a480959ba5fb4f65ff7f2b7.zip
Archive:  Tommy_2e00c18e3a480959ba5fb4f65ff7f2b7.zip
   creating: 300/
  inflating: 300/out.txt
  inflating: 300/parse
Megabeets:/tmp/h4ckit/canada# ll
total 628
drwxrwxrwx 2 root root      0 Oct  3 00:22 ./
drwxrwxrwx 2 root root      0 Oct  2 16:04 ../
drwxrwxrwx 2 root root      0 Sep 24 18:17 300/
-rwxrwxrwx 1 root root 634168 Sep 28 20:42 Tommy_2e00c18e3a480959ba5fb4f65ff7f2b7.zip*
Megabeets:/tmp/h4ckit/canada# strings /300/parse | grep -i h4ck1t{
...h4ck1t{T0mmy_g0t_h1s_Gun}...
```


I don&#8217;t even know what the challenge is about. Just moved to the next challenge without asking any unnecessary questions.

**Flag: **_h4ck1t{T0mmy\_g0t\_h1s_Gun}_



 [1]: http://ctf.com.ua/data/attachments/Tommy_2e00c18e3a480959ba5fb4f65ff7f2b7.zip