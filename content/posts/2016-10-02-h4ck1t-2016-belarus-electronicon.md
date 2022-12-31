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

<img data-attachment-id="581" data-permalink="https://www.megabeets.net/h4ck1t-2016-belarus-electronicon/h4ck1t_belarus_1/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_belarus_1.png" data-orig-size="932,581" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_belarus_1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_belarus_1-300x187.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_belarus_1.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-581" src="http://www.megabeets.net/uploads/h4ck1t_belarus_1.png" alt="h4ck1t_belarus_1" width="932" height="581" srcset="https://www.megabeets.net/uploads/h4ck1t_belarus_1.png 932w, https://www.megabeets.net/uploads/h4ck1t_belarus_1-150x94.png 150w, https://www.megabeets.net/uploads/h4ck1t_belarus_1-300x187.png 300w, https://www.megabeets.net/uploads/h4ck1t_belarus_1-768x479.png 768w, https://www.megabeets.net/uploads/h4ck1t_belarus_1-800x499.png 800w" sizes="(max-width: 932px) 100vw, 932px" /> 

Looks bad and it crashed my browser. This text file was too big for it to handle. So I opened it on Notepad++ and it was&#8217;t any better:

<img data-attachment-id="583" data-permalink="https://www.megabeets.net/h4ck1t-2016-belarus-electronicon/h4ck1t_belarus_2/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_belarus_2.png" data-orig-size="1353,664" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_belarus_2" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_belarus_2-300x147.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_belarus_2-1024x503.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-583" src="http://www.megabeets.net/uploads/h4ck1t_belarus_2.png" alt="h4ck1t_belarus_2" width="1353" height="664" srcset="https://www.megabeets.net/uploads/h4ck1t_belarus_2.png 1353w, https://www.megabeets.net/uploads/h4ck1t_belarus_2-150x74.png 150w, https://www.megabeets.net/uploads/h4ck1t_belarus_2-300x147.png 300w, https://www.megabeets.net/uploads/h4ck1t_belarus_2-768x377.png 768w, https://www.megabeets.net/uploads/h4ck1t_belarus_2-1024x503.png 1024w, https://www.megabeets.net/uploads/h4ck1t_belarus_2-800x393.png 800w" sizes="(max-width: 1353px) 100vw, 1353px" /> 

Still terrifying and it was heavy for notepad++ also. But this time something catched my eye. Look at the rows panel on the left, it says only 1 line. Let&#8217;s cancel word wrap (View > Word wrap) and check what it is:

<img data-attachment-id="584" data-permalink="https://www.megabeets.net/h4ck1t-2016-belarus-electronicon/h4ck1t_belarus_3/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_belarus_3.png" data-orig-size="1271,176" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_belarus_3" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_belarus_3-300x42.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_belarus_3-1024x142.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-584" src="http://www.megabeets.net/uploads/h4ck1t_belarus_3.png" alt="h4ck1t_belarus_3" width="1271" height="176" srcset="https://www.megabeets.net/uploads/h4ck1t_belarus_3.png 1271w, https://www.megabeets.net/uploads/h4ck1t_belarus_3-150x21.png 150w, https://www.megabeets.net/uploads/h4ck1t_belarus_3-300x42.png 300w, https://www.megabeets.net/uploads/h4ck1t_belarus_3-768x106.png 768w, https://www.megabeets.net/uploads/h4ck1t_belarus_3-1024x142.png 1024w, https://www.megabeets.net/uploads/h4ck1t_belarus_3-800x111.png 800w" sizes="(max-width: 1271px) 100vw, 1271px" /> 

Aah ah! It was a HUGE ascii-art. How huge? 11 rows of 1830661 chars each! It&#8217;s a long hex string. So now we need to parse it. I tried using [this][2] module but without any success so I decided to go for the hard way. I parsed it myself.

First, I edited the file in order to make it easy for me to parse it. I wanted that every char will be in it&#8217;s own line. I wrote a script to separate the characters:

<pre class="lang:python decode:true">import os

fin = open('pain.txt','r')
fout = open('out.txt', 'w')

content=fin.read()

splitted=content.split('\n')
width=13
	
print len(content)
for j in xrange(len(splitted[0])/width):
	for i in xrange(len(splitted)):
		fout.write(splitted[i][:width]+"\n")
		splitted[i]=splitted[i][width:]</pre>

Now let&#8217;s open the edited file with _EmEditor_ that is capable of open large files and see how our file is looking like:

<img data-attachment-id="585" data-permalink="https://www.megabeets.net/h4ck1t-2016-belarus-electronicon/h4ck1t_belarus_4/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_belarus_4.png" data-orig-size="193,662" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_belarus_4" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_belarus_4-87x300.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_belarus_4.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-585" src="http://www.megabeets.net/uploads/h4ck1t_belarus_4.png" alt="h4ck1t_belarus_4" width="193" height="662" srcset="https://www.megabeets.net/uploads/h4ck1t_belarus_4.png 193w, https://www.megabeets.net/uploads/h4ck1t_belarus_4-44x150.png 44w" sizes="(max-width: 193px) 100vw, 193px" /> 

Good! Looks exactly like I wanted! Now in order to parse it we need to tell the code how every letter or digit is looking like so I started to define variable for each letter or digit with the matching ascii-art. It was something like that:

<pre class="lang:python decode:true">f_in = open('out.txt', 'r')
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
     index+=1</pre>

I took the long hex-string and paste in hex editor. It was this photo:<img data-attachment-id="586" data-permalink="https://www.megabeets.net/h4ck1t-2016-belarus-electronicon/img/#main" data-orig-file="http://www.megabeets.net/uploads/img.jpg" data-orig-size="1300,219" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;1&quot;}" data-image-title="h4ck1t_belarus_5" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/img-300x51.jpg" data-large-file="http://www.megabeets.net/uploads/img-1024x173.jpg" decoding="async" loading="lazy" class="alignnone size-full wp-image-586" src="http://www.megabeets.net/uploads/img.jpg" alt="h4ck1t_belarus_5" width="1300" height="219" srcset="https://www.megabeets.net/uploads/img.jpg 1300w, https://www.megabeets.net/uploads/img-150x25.jpg 150w, https://www.megabeets.net/uploads/img-300x51.jpg 300w, https://www.megabeets.net/uploads/img-768x129.jpg 768w, https://www.megabeets.net/uploads/img-1024x173.jpg 1024w, https://www.megabeets.net/uploads/img-800x135.jpg 800w" sizes="(max-width: 1300px) 100vw, 1300px" />

Well, that&#8217;s it. We got the flag and we now can rest in peace.

**Flag:** _h4ck1t{1\_L0V3\_3P1C_F0NT$}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://ctf.com.ua/data/attachments/pain.txt
 [2]: https://github.com/eshel/asciiart