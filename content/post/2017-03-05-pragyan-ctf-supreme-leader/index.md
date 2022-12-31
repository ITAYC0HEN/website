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

<img src="../uploads/supreme_leader.png" /> 

Cute Kim 🙂

Now let&#8217;d dump the headers of the response using `curl`:

```diff
Megabeets$ curl -D - http://139.59.62.216/supreme_leader/
HTTP/1.1 200 OK
Date: Sun, 05 Mar 2017 08:47:14 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.20
Set-Cookie: KimJongUn=2541d938b0a58946090d7abdde0d3890_b8e2e0e422cae4838fb788c891afb44f; expires=Sun, 05-Mar-2017 08:47:24 GMT; Max-Age=10
Set-Cookie: KimJongUn=TooLateNukesGone; expires=Sun, 05-Mar-2017 08:47:25 GMT; Max-Age=10
Vary: Accept-Encoding
Content-Length: 1117
Content-Type: text/html
```


&nbsp;

We can see an interesting cookie:  _KimJongUn=**2541d938b0a58946090d7abdde0d3890_b8e2e0e422cae4838fb788c891afb44**_**f**. The value of the cookie is looking like 2 MD5 hashes combined with &#8220;_&#8221;. Let&#8217;s try to crack them online using my [favorite site][1].

<img src="../uploads/supreme_hash_Crack.png" /> 

That&#8217;s it! Here is the flag: **pragyanctf{send_nukes}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://hashkiller.co.uk/md5-decrypter.aspx