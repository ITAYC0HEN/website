---
title: '[Pragyan CTF] Interstellar'
author: Megabeets
type: post
date: 2017-03-05T07:30:32+00:00
url: /pragyan-ctf-interstellar/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - CTF
  - Forensics
  - Writeups

---
## Description:

> **Forensics 150 pts**
> 
> Dr. Cooper, on another one of his endless journeys encounter a mysterious planet . However when he tried to land on it, the ship gave way and he was left stranded on the planet . Desperate for help, he relays a message to the mothership containing the details of the people with him . Their HyperPhotonic transmission is 10 times the speed of light, so there is no delay in the message . However, a few photons and magnetic particles interefered with the transmission, causing it to become as shown in the picture . Can you help the scientists on the mothership get back the original image?
> 
> transmission.png

We are given with a photo, I opened it in Photoshop and saw that parts of it are transparent.

<img data-attachment-id="1024" data-permalink="https://www.megabeets.net/pragyan-ctf-interstellar/interstellar_transmission/#main" data-orig-file="http://www.megabeets.net/uploads/Interstellar_transmission.png" data-orig-size="1292,735" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="Interstellar_transmission" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/Interstellar_transmission-300x171.png" data-large-file="http://www.megabeets.net/uploads/Interstellar_transmission-1024x583.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1024" src="https://www.megabeets.net/uploads/Interstellar_transmission.png" alt="" width="1292" height="735" srcset="https://www.megabeets.net/uploads/Interstellar_transmission.png 1292w, https://www.megabeets.net/uploads/Interstellar_transmission-150x85.png 150w, https://www.megabeets.net/uploads/Interstellar_transmission-300x171.png 300w, https://www.megabeets.net/uploads/Interstellar_transmission-768x437.png 768w, https://www.megabeets.net/uploads/Interstellar_transmission-1024x583.png 1024w, https://www.megabeets.net/uploads/Interstellar_transmission-800x455.png 800w" sizes="(max-width: 1292px) 100vw, 1292px" /> 

&nbsp;

I grabbed Python and removed the Alpha layer from the image. The Alpha layer controls pixels&#8217; transparency.

<pre class="toolbar:2 show-lang:2 nums:false nums-toggle:false lang:python decode:true ">from PIL import Image
Image.open('transmission.png').convert('RGB').save('output.png')</pre>

We got the result with the flag:

<img data-attachment-id="1025" data-permalink="https://www.megabeets.net/pragyan-ctf-interstellar/2output-2/#main" data-orig-file="http://www.megabeets.net/uploads/2output-1.png" data-orig-size="1920,1080" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="transmission_output" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/2output-1-300x169.png" data-large-file="http://www.megabeets.net/uploads/2output-1-1024x576.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1025" src="https://www.megabeets.net/uploads/2output-1.png" alt="" width="1920" height="1080" srcset="https://www.megabeets.net/uploads/2output-1.png 1920w, https://www.megabeets.net/uploads/2output-1-150x84.png 150w, https://www.megabeets.net/uploads/2output-1-300x169.png 300w, https://www.megabeets.net/uploads/2output-1-768x432.png 768w, https://www.megabeets.net/uploads/2output-1-1024x576.png 1024w, https://www.megabeets.net/uploads/2output-1-800x450.png 800w" sizes="(max-width: 1920px) 100vw, 1920px" /> 

&nbsp;

The flag was **pragyanctf{Cooper_Brand}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>