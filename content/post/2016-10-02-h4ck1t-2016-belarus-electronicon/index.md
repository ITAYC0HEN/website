---
title: '[H4CK1T 2016] Belarus – Electronicon Writeup'
author: Megabeets
type: post
date: 2016-10-02T22:23:53+00:00
url: /h4ck1t-2016-belarus-electronicon/
categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - PPC
  - Writeups

---
### **Description:**

> **Belarus &#8211; Electronicon &#8211; PPC &#8211; 250 pts**  
> EN: This task is one of the methods for the psychological attacks. It is intended for people who don&#8217;t have heart diseases and reached 18 years 😉
> 
> h4ck1t{flag.upper()}
> 
> [paint.txt][1]

As the attached file says, it was real pain. I opened the file in the browser and saw this horrible thing:

<img src="../uploads/h4ck1t_belarus_1.png" /> 

Looks bad and it crashed my browser. This text file was too big for it to handle. So I opened it on Notepad++ and it was&#8217;t any better:

<img src="../uploads/h4ck1t_belarus_2.png" /> 

Still terrifying and it was heavy for notepad++ also. But this time something catched my eye. Look at the rows panel on the left, it says only 1 line. Let&#8217;s cancel word wrap (View > Word wrap) and check what it is:

<img src="../uploads/h4ck1t_belarus_3.png" /> 

Aah ah! It was a HUGE ascii-art. How huge? 11 rows of 1830661 chars each! It&#8217;s a long hex string. So now we need to parse it. I tried using [this][2] module but without any success so I decided to go for the hard way. I parsed it myself.

First, I edited the file in order to make it easy for me to parse it. I wanted that every char will be in it&#8217;s own line. I wrote a script to separate the characters:

```python
import os

fin = open('pain.txt','r')
fout = open('out.txt', 'w')

content=fin.read()

splitted=content.split('\n')
width=13
	
print len(content)
for j in xrange(len(splitted[0])/width):
	for i in xrange(len(splitted)):
		fout.write(splitted[i][:width]+"\n")
		splitted[i]=splitted[i][width:]
```


Now let&#8217;s open the edited file with _EmEditor_ that is capable of open large files and see how our file is looking like:

<img src="../uploads/h4ck1t_belarus_4.png" /> 

Good! Looks exactly like I wanted! Now in order to parse it we need to tell the code how every letter or digit is looking like so I started to define variable for each letter or digit with the matching ascii-art. It was something like that:

```python
f_in = open('out.txt', 'r')
ff = open('flag.txt', 'w')
content = f_in.read()
content = content.split("\n")

f = content[0:11] # The letter f
c8 = content[37:48] # The digit 8
... # Another letters and digits
... # Another letters and digits
... # Another letters and digits

index = 0
while True:
     lines=content[index*12:(index+1)*12-1]
     if lines==a:
             ff.write("a")
     elif lines==b:
             ff.write("b")
     elif lines==c:
             ff.write("c")
     elif lines==c0:
             ff.write("0")
     elif lines==c1:
             ff.write("1")
     elif lines==c2:
             ff.write("2")
     elif lines==c3:
             ff.write("3")
     elif lines==c4:
             ff.write("4")
     elif lines==c5:
             ff.write("5")
     elif lines==c6:
             ff.write("6")
     elif lines==c7:
             ff.write("7")
     elif lines==c8:
             ff.write("8")
     elif lines==c9:
             ff.write("9")
     elif lines==d:
             ff.write("d")
     elif lines==e:
             ff.write("e")
     elif lines==f:
             ff.write("f")
     index+=1
```


I took the long hex-string and paste in hex editor. It was this photo:<img src="../uploads/img.jpg" />

Well, that&#8217;s it. We got the flag and we now can rest in peace.

**Flag:** _h4ck1t{1\_L0V3\_3P1C_F0NT$}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://ctf.com.ua/data/attachments/pain.txt
 [2]: https://github.com/eshel/asciiart