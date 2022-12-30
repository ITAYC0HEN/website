---
title: '[Pragyan CTF] Supreme Leader'
author: Megabeets
type: post
date: 2017-03-05T07:31:37+00:00
url: /pragyan-ctf-supreme-leader/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - CTF
  - Web
  - Writeups

---
## Description:

> North Korea reportedly has a bioweapon in the making. Hack into their database and steal it.
> 
> Link : <http://139.59.62.216/supreme_leader>

For the second web challenge we&#8217;re given with a URL, lets open it.

<img data-attachment-id="1028" data-permalink="https://www.megabeets.net/pragyan-ctf-supreme-leader/supreme_leader/#main" data-orig-file="http://www.megabeets.net/uploads/supreme_leader.png" data-orig-size="900,589" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="supreme_leader" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/supreme_leader-300x196.png" data-large-file="http://www.megabeets.net/uploads/supreme_leader.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1028" src="https://www.megabeets.net/uploads/supreme_leader.png" alt="" width="900" height="589" srcset="https://www.megabeets.net/uploads/supreme_leader.png 900w, https://www.megabeets.net/uploads/supreme_leader-150x98.png 150w, https://www.megabeets.net/uploads/supreme_leader-300x196.png 300w, https://www.megabeets.net/uploads/supreme_leader-768x503.png 768w, https://www.megabeets.net/uploads/supreme_leader-800x524.png 800w" sizes="(max-width: 900px) 100vw, 900px" /> 

Cute Kim 🙂

Now let&#8217;d dump the headers of the response using `curl`:

<pre class="lang:diff decode:true ">Megabeets$ curl -D - http://139.59.62.216/supreme_leader/
HTTP/1.1 200 OK
Date: Sun, 05 Mar 2017 08:47:14 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.20
Set-Cookie: KimJongUn=2541d938b0a58946090d7abdde0d3890_b8e2e0e422cae4838fb788c891afb44f; expires=Sun, 05-Mar-2017 08:47:24 GMT; Max-Age=10
Set-Cookie: KimJongUn=TooLateNukesGone; expires=Sun, 05-Mar-2017 08:47:25 GMT; Max-Age=10
Vary: Accept-Encoding
Content-Length: 1117
Content-Type: text/html</pre>

&nbsp;

We can see an interesting cookie:  _KimJongUn=**2541d938b0a58946090d7abdde0d3890_b8e2e0e422cae4838fb788c891afb44**_**f**. The value of the cookie is looking like 2 MD5 hashes combined with &#8220;_&#8221;. Let&#8217;s try to crack them online using my [favorite site][1].

<img data-attachment-id="1029" data-permalink="https://www.megabeets.net/pragyan-ctf-supreme-leader/supreme_hash_crack/#main" data-orig-file="http://www.megabeets.net/uploads/supreme_hash_Crack.png" data-orig-size="753,181" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="supreme_hash_Crack" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/supreme_hash_Crack-300x72.png" data-large-file="http://www.megabeets.net/uploads/supreme_hash_Crack.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1029" src="https://www.megabeets.net/uploads/supreme_hash_Crack.png" alt="" width="753" height="181" srcset="https://www.megabeets.net/uploads/supreme_hash_Crack.png 753w, https://www.megabeets.net/uploads/supreme_hash_Crack-150x36.png 150w, https://www.megabeets.net/uploads/supreme_hash_Crack-300x72.png 300w" sizes="(max-width: 753px) 100vw, 753px" /> 

That&#8217;s it! Here is the flag: **pragyanctf{send_nukes}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://hashkiller.co.uk/md5-decrypter.aspx