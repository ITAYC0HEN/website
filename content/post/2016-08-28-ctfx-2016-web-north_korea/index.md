---
title: '[CTF(x) 2016 : WEB] north korea – 50 pts Writeup'
author: Megabeets
type: post
date: 2016-08-28T16:47:54+00:00
url: /ctfx-2016-web-north_korea/
categories:
  - CTF(x)
  - Writeups
tags:
  - CTF
  - CTF(x)
  - Web
  - Writeups

---
**Description:**  
_What is North Korea hiding?  
http://problems.ctfx.io:7002/_

Entering the URL I faced with only a sentence:  
“We, the Democratic People&#8217;s Republic of Korea, have developed a revolutionary new security standard. The West doesn&#8217;t stand a chance.”

That’s all? I took a look at the source code (ctrl+u) to see if something is hiding, and indeed I saw a hidden button and a simple script:

<pre class="lang:js decode:true">&lt;button hidden type="button"&gt;Retrieve nuclear codes&lt;/button&gt;
&lt;span&gt;&lt;/span&gt;
&lt;script type="text/javascript"&gt;
$(function() {
	$("button").click(function() {
		$.get('code', function(code) {
			$('span').text(code);
		});
	});
});
&lt;/script&gt;</pre>

<p style="text-align: left;">
  I clicked the button and it gave me the content of “http://problems.ctfx.io:7002/code” which was a message: “Nice try kiddo”.<br /> Well, I took a look again at the first message: “&#8230;The West doesn&#8217;t stand a chance.”. What about the north? What if i”ll set the X-Forwarded-For to <a href="https://en.wikipedia.org/wiki/Internet_in_North_Korea#IP_address_ranges">North Korea’s IP</a>? <a href="https://en.wikipedia.org/wiki/X-Forwarded-For" target="_blank">X-Forwarded-For </a>is the conventional way of identifying the originating IP address of the user connecting to the web server coming from either a HTTP proxy, load balancer.
</p>

<pre class="lang:sh decode:true">Curl --header 'X-Forwarded-For: 175.45.176.0' -i http://problems.ctfx.io:7002/code -k -L</pre>

And the response came with the flag:  
_ctf(jk\_we\_aint\_got\_n0_nuk35)_

<p style="text-align: right;">
  <img src="../uploads/megabeets_inline_logo.png" />Eat Veggies.
</p>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>