---
title: '[H4CK1T 2016] 1magePr1son- Mozambique Writeup'
author: Megabeets
type: post
date: 2016-10-03T00:10:42+00:00
url: /h4ck1t-2016-1magepr1son-mozambique-writeup/
categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - Stego
  - Writeups

---
### **Description:**

> **Task: 1magePr1son- Nozambique- Stego- 150** 
> 
> <span style="font-weight: 400;">Implementing of the latest encryption system as always brought a set of problems for one of the known FSI services: they have lost the module which is responsible for decoding information. And some information has been already ciphered! Your task for today: to define a cryptoalgorithm and decode the message.</span>  
> <span style="font-weight: 400;">https://ctf.com.ua/data/attachments/planet_982680d78ab9718f5a335ec05ebc4ea2.png.zip</span>  
> <span style="font-weight: 400;">h4ck1t{str(flag).upper()}</span>  
> [<span style="font-weight: 400;">https://ctf.com.ua/data/attachments/planet_982680d78ab9718f5a335ec05ebc4ea2.png.zip</span>][1]

For the start we are given with a wallpaper image named _planet.png_ (2560&#215;1850)

<img data-attachment-id="639" data-permalink="https://www.megabeets.net/h4ck1t-2016-1magepr1son-mozambique-writeup/h4ck1t_mozambiqu1/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu1.png" data-orig-size="512,316" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_mozambiqu1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu1-300x185.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu1.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-639" src="http://www.megabeets.net/uploads/h4ck1t_mozambiqu1.png" alt="h4ck1t_mozambiqu1" width="512" height="316" srcset="https://www.megabeets.net/uploads/h4ck1t_mozambiqu1.png 512w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu1-150x93.png 150w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu1-300x185.png 300w" sizes="(max-width: 512px) 100vw, 512px" /> 

Looking carefully at the image we can see a pattern of strange dots, such dots may be connected to the cryptosystem. Those are pixels in different colors that probably belongs to another image. My thought is that the pixels of the flag image was splitted into the wallpaper.

<img data-attachment-id="640" data-permalink="https://www.megabeets.net/h4ck1t-2016-1magepr1son-mozambique-writeup/h4ck1t_mozambiqu2/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu2.png" data-orig-size="925,416" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_mozambiqu2" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu2-300x135.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu2.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-640" src="http://www.megabeets.net/uploads/h4ck1t_mozambiqu2.png" alt="h4ck1t_mozambiqu2" width="925" height="416" srcset="https://www.megabeets.net/uploads/h4ck1t_mozambiqu2.png 925w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu2-150x67.png 150w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu2-300x135.png 300w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu2-768x345.png 768w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu2-800x360.png 800w" sizes="(max-width: 925px) 100vw, 925px" /> 

The dots exists every 24 pixels so I wrote a short pythons script in order to combine them into one image:

<pre class="lang:python decode:true ">from PIL import Image

original = Image.open("planet.png")
p_orig = original.load()
width, height = original.size
new_image = Image.new('RGBA',(width,height)) # The original image dimensions
p_flag = new_image.load()
cord_x, cord_y = 0, 0

# Collect the pixels and add them to the new image 
for j in range(0,height,24):
    for i in range(0,width,24):
        p_flag[cord_x,cord_y] = p_orig[i,j]
        cord_x+=1
    cord_y+=1
    cord_x=0
	
new_image.save('flag.png', 'PNG')
</pre>

I ran it and got a big image (the wallpaper size) with this tiny image inside that contains the flag:

<img data-attachment-id="641" data-permalink="https://www.megabeets.net/h4ck1t-2016-1magepr1son-mozambique-writeup/h4ck1t_mozambiqu3/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu3.png" data-orig-size="257,257" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_mozambiqu3" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu3.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_mozambiqu3.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-641" src="http://www.megabeets.net/uploads/h4ck1t_mozambiqu3.png" alt="h4ck1t_mozambiqu3" width="257" height="257" srcset="https://www.megabeets.net/uploads/h4ck1t_mozambiqu3.png 257w, https://www.megabeets.net/uploads/h4ck1t_mozambiqu3-150x150.png 150w" sizes="(max-width: 257px) 100vw, 257px" /> 

**Flag: **_h4ck1t{SPACE\_IS\_THE_KEY}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://ctf.com.ua/data/attachments/planet_982680d78ab9718f5a335ec05ebc4ea2.png.zip