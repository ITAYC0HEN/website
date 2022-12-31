---
title: '[Pragyan CTF] New Avenger'
author: Megabeets
type: post
date: 2017-03-05T07:32:29+00:00
url: /pragyan-ctf-new-avenger/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - CTF
  - Stego
  - Writeups

---
## Description:

> <div class="challenge-description">
>   <strong>New Avenger |</strong> Stego 300 pts
> </div>
> 
> <div class="challenge-description">
>   The Avengers are scouting for a new member. They have travelled all around the world, looking for suitable candidates for the new position.<br /> Finally, they have found the perfect candidate. But, they are in a bad situation. They do not know who the guy is behind the mask.<br /> Can you help the Avengers to uncover the identity of the person behind the mask ?
> </div>
> 
> <div class="challenge-description">
>
> </div>
> 
> <div class="challenge-files">
>   <div>
>     <span class="challenge-attachment"><a class="has-tooltip" title="" href="https://ctf.pragyan.org/download?file_key=759243671b88bc6c3024e12c1fa580fb4017e7e93f70f0bbe4cbaf3e1ed293bc&team_key=a500afc4a171f394f280518fefd78d62f976bf8303f77f3431573fce01c983cb" data-toggle="tooltip" data-placement="right" data-original-title="2.57 MB">avengers.gif</a> </span>
>   </div>
> </div>

<div>
</div>

<div>
  Those of you who read my blog frequently are already know how much I&#8217;m into superheroes. Give me a challenge with superheroes and you bought me. Although I&#8217;m more DC guy, this challenge was with the Marvels and still it was awesome! We&#8217;re given with a gif file. I ran `binwalk` on it to find whether it contains another files within.
</div>

<div>
  <pre class="lang:diff decode:true ">Megabeets$ binwalk avengers.gif

DECIMAL         HEX             DESCRIPTION
-------------------------------------------------------------------------------------------------------
0               0x0             GIF image data, version 8"9a", 500 x 272
885278          0xD821E         Zip archive data, at least v2.0 to extract, compressed size: 13422, uncompressed size: 13780, name: "1_image.jpg"
898769          0xDB6D1         Zip archive data, at least v1.0 to extract, compressed size: 1796904, uncompressed size: 1796904, name: "image_2.zip"</pre>
  
  <p>
    Yep, the gif file contains two more files within, lets unzip the image:
  </p>
  
  <pre class="lang:diff decode:true ">Megabeets$ unzip ./avengers.gif
Archive:  avengers.gif
warning [avengers.gif]:  885278 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: 1_image.jpg
 extracting: image_2.zip</pre>
  
  <p>
    Nice! We now have two more files:<em> image_2.zip</em> and <em>1_image.jpg. </em>Now lets try to unzip <em>image_2.zip.</em>
  </p>
  
  <pre class="lang:diff decode:true ">Megabeets$ unzip ./image_2.zip
Archive:  image_2.zip
[image_2.zip] 2_image.jpg password:</pre>
  
  <p>
    Oh-no, it requires a password. Lets have a look at <em>1_image.jpg.<img src="../uploads/1_image.jpg" /><br /> </em>Haha, funny image. Now I want to have a deeper look at this picture, I opened it in hex editor and found the password:
  </p>
  
  <p>
    <img src="../uploads/hex.png" />
  </p>
  
  <p>
    So the password is <em>&#8220;sgtgFhswhfrighaulmvCavmpsb&#8221;</em>, lets unzip the file:
  </p>
  
  <pre class="toolbar:2 show-lang:2 lang:diff decode:true ">Megabeets$ unzip ./image_2.zip
Archive:  image_2.zip
[image_2.zip] 2_image.jpg password: &lt;em&gt;sgtgFhswhfrighaulmvCavmpsb&lt;/em&gt;
  inflating: 2_image.jpg
 extracting: image_3.zip</pre>
  
  <p>
    Again?! We got 2 more files, and the password to the new zip was at the end of the new image, and the new zip contained another zip and an image. Well, I see where it going to, so I opened python and automate the process:
  </p>
  
  <pre class="lang:python decode:true ">from zipfile import ZipFile
import string

# list storing the passwords, it might help 
passwords =[]
i = 1

while True:
    # read the last line of the file
    f = reversed(open("%s_image.jpg"%i).readlines())
    passw = f.next()
    try:
        # extract the password from the last line, if failed - it's the last zip.
        passw = passw[passw.index(':- ')+3:passw.index(' \n')]
    except:
        break
    # extract the zip file using the password
    with ZipFile('image_%s.zip'%(i+1)) as zf:
        zf.extractall(pwd=passw)
    i+=1    # add the password to the list of passwords
    passwords.append(passw)</pre>
  
  <p>
    Ta-dah! We extracted all the zip files and gםt 16 images and 15 passwords. This was the last image:
  </p>
  
  <p>
    <img src="../uploads/16_image.jpg" />
  </p>
</div>

lol.

So now we have 15 passwords, each contains 26 characters:

<pre class="lang:diff decode:true">sgtgFhswhfrighaulmvCavmpsb
lppujmioEaynaqrctesAnztgib
lrphntGpzjhkswskepnilrwwjm
hmohAmgcomgpjjhLnqpkepuazi
qjqxzuSkiyjzazwwsqchiqvgoQ
ujinpqyghiulozjnyprZpnswnp
tsquviQwxtgpgarlxelvakaOpo
jljykvfZSycpvscqvzjwelKhok
cqjausmhroogiuabcbpRmsyzpo
qakrlxrGswfovmxhxpjzfyfrie
jyxbLszctbveelbgxtilzfbQng
heojthirkakqvvmxjgAWzuekcp
nkpbhyUmiabnymvzmcppejiisy
mIsmsmsmxpfvkolTbnkafkgvgx
tsYinxviqeykguqznjscomgqbh</pre>

The password looks like garbage, it&#8217;s not Base64 or some known encoding. The first thing to pop up is the capital letter inside each password. Every password contains one or two capital letters. I know that the English alphabet contains 26 letters, so maybe I can map the location of each capital to the matching letter in the alphabet. i.e, if &#8216;F&#8217; is in `passw[4]` i&#8217;ll take `alphabet[4]` which is &#8216;e&#8217; and so on. I added this code to my script:

<pre class="toolbar:2 show-lang:2 nums:false nums-toggle:false lang:python decode:true">locations = []
for p in passwords:
    for c in range(26):
        if p[c] in string.uppercase:
            locations.append(c)

map_result = ''
for l in locations:
    map_result += string.lowercase[l]

print "Result: ", map_result
#Result:  etitgepgztgxhiwthexstgbpc</pre>

I ran the script and got meaningless string: _&#8220;etitgepgztgxhiwthexstgbpc&#8221;. _Damn! I was so sure that the mapping is the solution, how can&#8217;t it be?! All the facts point towards mapping the alphabet. I decided not to give up and ran Caesar Cipher on the string:

<img src="../uploads/peter.png" /> 

YAY! I was so happy to find Spidey is the new Avenger!

Here&#8217;s the full script:



The flag was: **pragyanctf{peterparkeristhespiderman}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>