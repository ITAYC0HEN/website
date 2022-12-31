---
title: '[TWCTF-2016: Web] Rescue Data 1: deadnas Writeup'
author: Megabeets
type: post
date: 2016-09-05T00:00:18+00:00
url: /twctf-2016-web-rescue-data-1-deadnas/
categories:
  - TWCTF 2016
  - Writeups
tags:
  - CTF
  - Forensics
  - TWCTF
  - Writeups

---
> **Challenge description:**
> 
> _Today, our 3-disk NAS has failed. Please recover flag._  
> _[deadnas.7z][1]{.attachment}_

* * *

We are given an archive containing 3 files:

<pre class="lang:sh decode:true">D:\Megabeets\deadnas&gt; dir 
Directory of D:\Megabeets\deadnas
        .
        ..
524,288 disk0
     12 disk1
524,288 disk2</pre>

3 Disk NAS and one has failed? This challenge is obviously about [RAID 5][2]. I was asked to find a way to recover the failed disk and there is no simpler way than just XOR disk0 with disk2 and recreate the original disk1. If you are right now in your &#8220;WTF?!&#8221; mode you better go read about RAID 5 until you understand how it works.

I used simple software called [XorFiles][3].

<img data-attachment-id="180" data-permalink="https://www.megabeets.net/twctf-2016-web-rescue-data-1-deadnas/xorfiles/#main" data-orig-file="http://www.megabeets.net/uploads/XorFiles.png" data-orig-size="583,194" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="XorFiles" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/XorFiles-300x100.png" data-large-file="http://www.megabeets.net/uploads/XorFiles.png" decoding="async" loading="lazy" class="alignnone wp-image-180 size-full" src="http://megabeets.net/uploads/XorFiles.png" alt="XorFiles" width="583" height="194" srcset="https://www.megabeets.net/uploads/XorFiles.png 583w, https://www.megabeets.net/uploads/XorFiles-150x50.png 150w, https://www.megabeets.net/uploads/XorFiles-300x100.png 300w" sizes="(max-width: 583px) 100vw, 583px" /> 

I then used OSForensics to rebuild the RAID:

<img data-attachment-id="189" data-permalink="https://www.megabeets.net/twctf-2016-web-rescue-data-1-deadnas/osforensics/#main" data-orig-file="http://www.megabeets.net/uploads/OSForensics.png" data-orig-size="618,312" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="OSForensics" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/OSForensics-300x151.png" data-large-file="http://www.megabeets.net/uploads/OSForensics.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-189" src="http://megabeets.net/uploads/OSForensics.png" alt="OSForensics" width="618" height="312" srcset="https://www.megabeets.net/uploads/OSForensics.png 618w, https://www.megabeets.net/uploads/OSForensics-150x76.png 150w, https://www.megabeets.net/uploads/OSForensics-300x151.png 300w" sizes="(max-width: 618px) 100vw, 618px" /> 

Mounted the output file:

<img data-attachment-id="191" data-permalink="https://www.megabeets.net/twctf-2016-web-rescue-data-1-deadnas/osforensics2/#main" data-orig-file="http://www.megabeets.net/uploads/OSForensics2.png" data-orig-size="346,529" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="OSForensics2" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/OSForensics2-196x300.png" data-large-file="http://www.megabeets.net/uploads/OSForensics2.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-191" src="http://megabeets.net/uploads/OSForensics2.png" alt="OSForensics2" width="346" height="529" srcset="https://www.megabeets.net/uploads/OSForensics2.png 346w, https://www.megabeets.net/uploads/OSForensics2-98x150.png 98w, https://www.megabeets.net/uploads/OSForensics2-196x300.png 196w" sizes="(max-width: 346px) 100vw, 346px" /> 

And accessed the new drive. The flag and a cute cat was waiting for me there.

<img data-attachment-id="190" data-permalink="https://www.megabeets.net/twctf-2016-web-rescue-data-1-deadnas/globalpage_flag/#main" data-orig-file="http://www.megabeets.net/uploads/GlobalPage_Flag.png" data-orig-size="834,663" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="GlobalPage_Flag" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/GlobalPage_Flag-300x238.png" data-large-file="http://www.megabeets.net/uploads/GlobalPage_Flag.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-190" src="http://megabeets.net/uploads/GlobalPage_Flag.png" alt="GlobalPage_Flag" width="834" height="663" srcset="https://www.megabeets.net/uploads/GlobalPage_Flag.png 834w, https://www.megabeets.net/uploads/GlobalPage_Flag-150x119.png 150w, https://www.megabeets.net/uploads/GlobalPage_Flag-300x238.png 300w, https://www.megabeets.net/uploads/GlobalPage_Flag-768x611.png 768w, https://www.megabeets.net/uploads/GlobalPage_Flag-800x636.png 800w" sizes="(max-width: 834px) 100vw, 834px" /> 

<span style="font-size: 10pt; color: #ff0000;">* I know you tried using <strong>mdadm</strong> and <strong>ReclaiMe</strong>. Poor you.</span>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://twctf7qygt6ujk.azureedge.net/uploads/deadnas.7z-b1651b1230b507235cbb9c6f7e98ccc437f5f3675d02a5e70951e2cbcf9df407
 [2]: http://blog.open-e.com/how-does-raid-5-work/
 [3]: http://www.nirsoft.net/utils/xorfiles.html