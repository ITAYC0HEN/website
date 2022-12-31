---
title: '[ASIS CTF] CTF 101 Writeup'
author: Megabeets
type: post
date: 2016-09-11T17:00:43+00:00
url: /asis-ctf-ctf-101/
categories:
  - ASIS CTF 2016
  - Writeups
tags:
  - ASIS-CTF
  - CTF

---
> _**Description:**http://www.megabeets.net/wp-admin/profile.php  
> Watch your heads!_

The description is telling the whole story. Simply look in the response&#8217;s header and you&#8217;ll find the flag. In order to do that open the browser&#8217;s Developer Tools (F12), bring to focus the Network tab and click the challenge. The HTTP requests will show up on the left panel. Select the request and the **Flag** header will be displayed on the right panel.

<div class="header-name">
  <img data-attachment-id="289" data-permalink="https://www.megabeets.net/asis-ctf-ctf-101/asisct101flag/#main" data-orig-file="http://www.megabeets.net/uploads/asisct101flag.png" data-orig-size="722,394" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="asisct101flag" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/asisct101flag-300x164.png" data-large-file="http://www.megabeets.net/uploads/asisct101flag.png" decoding="async" loading="lazy" class="aligncenter wp-image-289 size-full" style="-webkit-box-shadow: 0px 0px 33px -12px rgba(0,0,0,0.5); -moz-box-shadow: 0px 0px 33px -12px rgba(0,0,0,0.5); box-shadow: 0px 0px 33px -12px rgba(0,0,0,0.5);" src="http://www.megabeets.net/uploads/asisct101flag.png" width="722" height="394" srcset="https://www.megabeets.net/uploads/asisct101flag.png 722w, https://www.megabeets.net/uploads/asisct101flag-150x82.png 150w, https://www.megabeets.net/uploads/asisct101flag-300x164.png 300w" sizes="(max-width: 722px) 100vw, 722px" />
</div>

&nbsp;

Decode the string with base64 and reveal the flag.

<pre class="lang:sh decode:true">$ echo QVNJU3szMWE0ODM5MDBiODU3NjQyNmNjY2RmNTU0MDJiOWRkNn0K | base64 --decode
ASIS{31a483900b8576426cccdf55402b9dd6}
</pre>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>