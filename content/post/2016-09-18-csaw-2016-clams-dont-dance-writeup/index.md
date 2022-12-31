---
title: '[CSAW 2016] Clams Don’t Dance Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:01:40+00:00
url: /csaw-2016-clams-dont-dance-writeup/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - Forensics
  - Writeups

---
### **Description:**

> Find the clam and open it to find the pearl.  
> <a class="chal-file" href="https://ctf.csaw.io/static/uploads/dd1c652598c176078e3b558a01f5d9a2/out.img" target="_blank">out.img</a>

We are given with a file. I ran `file` command on it to figure it&#8217;s file type:

<pre class="lang:sh decode:true">[Megabeets] /tmp/CSAW/clam# file out.img
out.img: x86 boot sector</pre>

Ok, we have raw image file which will probably contain file/s with the flag. I&#8217;ll show 2 methods, choose your preferred one.

### **Method 1: Autopsy**

Open [Autopsy][1] (my favorite forensics software, it&#8217;s free and heartily recommended) and choose the &#8220;Create New Case&#8221; on the Welcome window. You can also create a new case from the &#8220;File&#8221; menu.

<img data-attachment-id="381" data-permalink="https://www.megabeets.net/csaw-2016-clams-dont-dance-writeup/autopsy_open/#main" data-orig-file="http://www.megabeets.net/uploads/autopsy_open.png" data-orig-size="450,316" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="autopsy_open" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/autopsy_open-300x211.png" data-large-file="http://www.megabeets.net/uploads/autopsy_open.png" decoding="async" loading="lazy" class="size-full wp-image-381 aligncenter" src="http://www.megabeets.net/uploads/autopsy_open.png" alt="autopsy_open" width="450" height="316" srcset="https://www.megabeets.net/uploads/autopsy_open.png 450w, https://www.megabeets.net/uploads/autopsy_open-150x105.png 150w, https://www.megabeets.net/uploads/autopsy_open-300x211.png 300w" sizes="(max-width: 450px) 100vw, 450px" /> 

Fill the requested details and press &#8220;Finish&#8221;. Now add input data source to the case. Click &#8220;Case&#8221; > &#8220;Add Data Source&#8230;&#8221; or on the &#8220;Add Data Source&#8221; button (  <img data-attachment-id="383" data-permalink="https://www.megabeets.net/csaw-2016-clams-dont-dance-writeup/autopsy_add_data_source/#main" data-orig-file="http://www.megabeets.net/uploads/Autopsy_Add_Data_Source.png" data-orig-size="118,32" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="autopsy_add_data_source" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/Autopsy_Add_Data_Source.png" data-large-file="http://www.megabeets.net/uploads/Autopsy_Add_Data_Source.png" decoding="async" loading="lazy" class="wp-image-383 alignnone" src="http://www.megabeets.net/uploads/Autopsy_Add_Data_Source.png" alt="autopsy_add_data_source" width="93" height="22" /> ) on the main window and choose our file, `out.img`.

I cannot teach you about all the features of this great software so I&#8217;ll just show the path to the flag. If you&#8217;re not familiar with Autopsy I recommend you again to start working with it.

In the right panel you can find plenty of features, I will use only the directory viewer. Clicking on the data source (out.img) will show the files in the main directory of the image.

<img data-attachment-id="385" data-permalink="https://www.megabeets.net/csaw-2016-clams-dont-dance-writeup/autopsy_view_dir/#main" data-orig-file="http://www.megabeets.net/uploads/autopsy_view_dir.png" data-orig-size="785,605" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="autopsy_view_dir" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/autopsy_view_dir-300x231.png" data-large-file="http://www.megabeets.net/uploads/autopsy_view_dir.png" decoding="async" loading="lazy" class=" wp-image-385 aligncenter" src="http://www.megabeets.net/uploads/autopsy_view_dir.png" alt="autopsy_view_dir" width="679" height="484" /> 

&nbsp;

Do you see it? As it says in the description, we found the clam(.pptx)! The X on it&#8217;s icon means that this file was deleted from the  operation system (but not from the disk). Double clicking it and you&#8217;ll see bunch of image files, one of them, called `image0.gif`, is looking like a MaxiCode. Is it?

<img data-attachment-id="386" data-permalink="https://www.megabeets.net/csaw-2016-clams-dont-dance-writeup/clam_image0/#main" data-orig-file="http://www.megabeets.net/uploads/clam_image0.gif" data-orig-size="256,243" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="clam_image0" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/clam_image0.gif" data-large-file="http://www.megabeets.net/uploads/clam_image0.gif" decoding="async" loading="lazy" class="size-full wp-image-386 aligncenter" src="http://www.megabeets.net/uploads/clam_image0.gif" alt="clam_image0" width="256" height="243" /> 

Scan it either online or offline to reveal the flag. I was scanning it with [this][2] site.  
`flag{TH1NK ABOUT 1T B1LL. 1F U D13D, WOULD ANY1 CARE??}`

&nbsp;

### **Method 2: Foremost**

This is less elegant way to solve the challenge. Run `foremost` on the file:

<pre class="lang:sh decode:true ">[Megabeets]$ foremost out.img</pre>

You&#8217;ll get a folder named output with zip file, movie file and pptx file. Extract the pptx file using 7-zip (PPTX is an archive file), go to the `/ppt/media` folder and you&#8217;ll find the MaxiCode image mentioned before.

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: http://www.sleuthkit.org/autopsy/
 [2]: https://zxing.org/w/decode.jspx