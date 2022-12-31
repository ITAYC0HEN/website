---
title: '[H4CK1T 2016] Hex0gator – Paraguay Writeup'
author: Megabeets
type: post
date: 2016-10-02T20:33:31+00:00
url: /h4ck1t-2016-hex0gator-paraguay/
categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - PPC
  - Writeups

---
### **Description:**

> EN: All Experts of The Silver Shield Project can&#8217;t decipher the intercepted data. Who knows, maybe you can do it?  
> [<span style="font-weight: 400;">100_00edb54bed7e46bd5cdb7c06059881c2</span>][1]

&nbsp;

In this PPC 250 pts challenge we got only one file. Let&#8217;s run _File_ command on it to determine it&#8217;s type.

<pre class="lang:sh decode:true">Megabeets:/tmp/h4ckit/paraguay# file 100_00edb54bed7e46bd5cdb7c06059881c2
100_00edb54bed7e46bd5cdb7c06059881c2: Zip archive data, at least v2.0 to extract</pre>

&nbsp;

This is a zip file which contains another folder within. The folder contains a file named &#8216;_99_&#8216;. Let&#8217;s extract it and figure out it&#8217;s type:

<pre class="lang:sh decode:true">Megabeets:/tmp/h4ckit/paraguay# file 99
99: Zip archive data, at least v1.0 to extract</pre>

99 is also a zip file, and inside it has another zip, and another zip&#8230; well, I see where it going to. I wrote a simple Powershell script to extract all the archives using the ultimate archive manipulator &#8211; 7-zip.

<pre class="lang:ps decode:true "># Set $path to a folder only with the file '99'
# 99 Exists in 'work_folder' inside the first archive

$path = "C:\\your\\\path"

while($true)
{
    $file = (gci $path)[0]
    &'C:\Program Files\7-Zip\7z.exe' e $file.Fullname -y &gt; $null
    if($file.Name -eq 'flag')
    {
        # print the content of the file
        gc $file
        break;

    }
    else
    {
        Remove-Item $file.Fullname
    }
}</pre>

Now let&#8217;s run it:

<pre class="lang:ps decode:true">PS C:\h4ckit\paraguay&gt; C:\h4ckit\paraguay\solve.ps1
FLAG: 0W_MY_G0D_Y0U_M4D3_1T</pre>

&nbsp;

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://ctf.com.ua/data/attachments/100_00edb54bed7e46bd5cdb7c06059881c2