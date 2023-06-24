---
title: '[TWCTF-2016: Web] Global Page Writeup'
author: Megabeets
type: posts
date: 2016-09-05T00:01:54+00:00

categories:
  - TWCTF 2016
  - Writeups
tags:
  - CTF
  - LFI
  - TWCTF
  - Web
  - Writeups

---
> **Challenge description: **  
> _Welcome to TokyoWesterns&#8217; [CTF][1]!_

* * *

<span style="font-weight: 400;">As I entered the challenge I faced a three items list &#8211; two links and a strikethrough word:</span>.

  * <span style="font-size: 10pt;"><em><a href="http://globalpage.chal.ctf.westerns.tokyo/?page=tokyo">Tokyo</a></em></span>
  * <span style="font-size: 10pt;"><em><del>Westerns</del></em></span>
  * <span style="font-size: 10pt;"><em><a href="http://globalpage.chal.ctf.westerns.tokyo/?page=ctf">CTF</a></em></span>

<span style="font-weight: 400;">I clicked the </span>_<span style="font-weight: 400;">tokyo</span>_ <span style="font-weight: 400;">link, which was actually a GET request with a parameter named </span>_<span style="font-weight: 400;">page </span>_<span style="font-weight: 400;">in index.php. In response I got a page with PHP error and information from Wikipedia about Tokyo, printed in Hebrew &#8211; my mother tongue.</span>

<table>
  <tr>
    <td style="text-align: left;">
      <span style="font-size: 10pt;"><b>Warning</b>: include(tokyo/en-US.php): failed to open stream: No such file or directory in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span><br /> <span style="font-size: 10pt;"> <b>Warning</b>: include(): Failed opening &#8216;tokyo/en-US.php&#8217; for inclusion (include_path=&#8217;.:/usr/share/php:/usr/share/pear&#8217;) in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span><br /> &#8230;
    </td>
  </tr>
</table>

<span style="font-weight: 400;">First thing to come in mind is a <a href="https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion" target="_blank">LFI </a>attack, but before making any reckless time-wasting moves, let&#8217;s first figure it all out. The page uses </span><a href="http://php.net/manual/en/function.include.php" target="_blank"><span style="font-weight: 400;">include()</span></a> <span style="font-weight: 400;">to, well, include the page &#8220;</span>_<span style="font-weight: 400;">en-US.php</span>_<span style="font-weight: 400;">&#8221; from folder named </span>_<span style="font-weight: 400;">tokyo</span>_<span style="font-weight: 400;">. The page wasn&#8217;t existed so an error was thrown. I tried pages like &#8220;en.php&#8221;, &#8220;he.php&#8221; and &#8220;jp.php&#8221; and they did exist. The page &#8220;</span>_<span style="font-weight: 400;">ctf</span>_<span style="font-weight: 400;">&#8221; displayed similar behaviors. Seems like all the pages display their information based on the user&#8217;s or the browser&#8217;s language. </span>

<span style="font-weight: 400;">The second thing I tested was the page’s reactions to different values. I tried the value &#8220;</span>_<span style="font-weight: 400;">?page=flag</span>_<span style="font-weight: 400;">&#8221; and it returned the expected error:</span>

<table>
  <tr>
    <td>
      <span style="font-size: 10pt;"><b>Warning</b>: include(flag/en-US.php): failed to open stream: No such file or directory in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span><br /> <span style="font-size: 10pt;"> <b>Warning</b>: include(): Failed opening &#8216;flag/en-US.php&#8217; for inclusion (include_path=&#8217;.:/usr/share/php:/usr/share/pear&#8217;) in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span><br /> <span style="font-size: 10pt;"> <b>Warning</b>: include(flag/en.php): failed to open stream: No such file or directory in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span><br /> <span style="font-size: 10pt;"> <b>Warning</b>: include(): Failed opening &#8216;flag/en.php&#8217; for inclusion (include_path=&#8217;.:/usr/share/php:/usr/share/pear&#8217;) in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span>
    </td>
  </tr>
</table>

<span style="font-weight: 400;">I then understood the page was trying to include the language file and every value that I&#8217;ll set to &#8220;</span>_<span style="font-weight: 400;">page&#8221;</span>_ <span style="font-weight: 400;">will be a folder. I tested the page with the value &#8220;../../../etc/passwd&#8221; with and without a null-byte terminator but failed due to the sanitize of dots and slashes the page performs. </span>

<span style="font-weight: 400;">But how does the page know my language? It took me a while to figure it out. The page took my language settings from the &#8220;</span>_<span style="font-weight: 400;">Accept-Language</span>_<span style="font-weight: 400;">&#8221; field in the request&#8217;s header. I tried to change </span>_<span style="font-weight: 400;">Accept-Language</span>_ <span style="font-weight: 400;">to something else using a Firefox plugin called </span>_<span style="font-weight: 400;">Tamper Data</span>_ <span style="font-weight: 400;">and it worked! Any value I&#8217;ll put there will change the requested page. For example if I request &#8220;?page=Mega&#8221; and set </span>_<span style="font-weight: 400;">Accept-Language</span>_ <span style="font-weight: 400;">to &#8220;beets&#8221; it would return the errors:</span>

<table>
  <tr>
    <td>
      <span style="font-size: 10pt;"> <b>Warning</b>: include(Mega/beets.php): failed to open stream: No such file or directory in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span><br /> <span style="font-size: 10pt;"> <b>Warning</b>: include(): Failed opening &#8216;Mega/beets.php&#8217; for inclusion (include_path=&#8217;.:/usr/share/php:/usr/share/pear&#8217;) in <b>/var/www/globalpage/index.php</b> on line <b>41</b></span>
    </td>
  </tr>
</table>

<span style="font-weight: 400;">I combined it all together to perform a well known LFI attack using <a href="http://php.net/manual/en/wrappers.php.php" target="_blank">php://filter</a>. I set the parameter value to &#8220;php:&#8221; and the </span>_<span style="font-weight: 400;">Accept-Language</span>_ <span style="font-weight: 400;">field to &#8220;/filter/convert.base64-encode/resource=index&#8221;. This function encodes the page with Base64 before including it. And indeed I got &#8220;index.php&#8221; encoded with base64. The decoded page looks like this: </span>  


<span style="font-weight: 400;">As you can see on the top of the code there is an included page named &#8220;flag.php&#8221;. I changed the </span>_<span style="font-weight: 400;">Accept-Language </span>_<span style="font-weight: 400;">accordingly to &#8220;/filter/convert.base64-encode/resource=flag&#8221; and received the encoded page. Decode it to reveal the flag:</span>

**TWCTF{I\_found\_simple_LFI}**

<span style="font-size: 12pt;">By the way, you can also solve it the &#8220;Curl&#8221; way:</span>

```sh
curl 'http://globalpage.chal.ctf.westerns.tokyo/?page=php:' -H "Accept-Language:/filter/convert.base64-encode/resource=flag"
```


&nbsp;





 [1]: http://globalpage.chal.ctf.westerns.tokyo/