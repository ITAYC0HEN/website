---
title: '[33C3 CTF] pay2win Writeup'
author: Megabeets
type: post
date: 2016-12-29T20:01:12+00:00
url: /33c3-ctf-pay2win-writeup/
categories:
  - 33C3 CTF
  - Writeups
tags:
  - 33C3
  - CTF
  - Web
  - Writeups

---
### **Description:**

> pay2win &#8211; Web  
> Do you have enough money to buy the [flag][1]?

<div class="panel panel-primary">
</div>

This challenge was pretty tricky to understand at the beginning. I solved it with a quick and simple workaround that allowed me to solve the challenge without fully understand it. Once I got the flag I understood the whole story. So as with all the stories, we need to begin from the start.

We&#8217;re given with a website in where we can buy two products: &#8216;cheap&#8217; (13.37 USD) and &#8216;flag&#8217; (31337.42 USD). We, of course, want to buy the &#8216;cheap&#8217; one because we don&#8217;t want to spend our money on some leet flag with the answer to life, the universe and blah blah. So &#8212; the &#8216;cheap&#8217; it is.

<img src="../uploads/pay2win_1.png" /> 

In order to buy the product we need to supply a valid credit card number, there are bunch of examples of valid credit cards online.

<img src="../uploads/pay2win_2-1.png" /> 

Lets try one of them and see what we get.

<img src="../uploads/pay2win_3-1.png" /> 

Woo-hoo! We finally bought the &#8216;cheap&#8217; product and fulfilled our dream.  
Kidding. Lets move on and see what will we get when trying to buy the &#8216;flag&#8217;.

<img src="../uploads/pay2win_4.png" /> 

<img src="../uploads/pay2win_5-1.png" /> 

&#8220;failed&#8221;? Oh no. The server says that we exceeded the credit card limit. The first thing to come in my mind was to brute force the server with valid CC numbers, but I figured out very fast that this isn&#8217;t the right way to the solution. At this time I noticed something interesting about the URLs of the pages: there&#8217;s a GET parameter named &#8216;data&#8217; that some parts of it are the same on every request. Until now I thought it&#8217;s always a new hash. I grabbed pencil and paper and started to figure out the patterns and the mutual parts. Okay, okay, I admit &#8211; opened VS Code and made a simple table. The mutual parts highlighted using Photoshop.

[<img src="../uploads/pay2win_6.png" />][2]

As you can see, every hash is combined from 3 parts. The beginning of each type is mutual and so is the end. I thought that certain combination is required to get the flag. But how I mix the parts to the correct hash so as to get the &#8216;flag&#8217; content. Now it&#8217;s about trial and error. **Or not**.

After two manual tries I gave up because automation is always better and here comes the workaround I mentioned before. I created a list with instance of every colored part and added one example of white part from each page. I then created from this list another list with all possible permutations of 3 parts, i.e all the possible combinatios (990 combinations) and tried all of them using `urllib2.urlopen()` &#8217;til I found &#8217;33C3&#8242; in the response.

I know. It isn&#8217;t the most efficient way to do this but it was short and quick.

```python
import itertools
import urllib2

hash_parts = ['28df361f896eb3c3706cda0474915040',
'5e4ec20070a567e096d3b89ed5a54b1d',
'23b5b0554edda4f8828df361f896eb3c3706cda0474915040',
'4f75c9736d3b8e0641e7995bb92506da1ac7f8da5a628e19ae39825a916d8a2f',
'232c66210158dfb23a2eda5cc945a0a9650c1ed0fa0a08f6',
'2f7ef761e2bbe791',
'47aae22e7d77d379272d81aff52de2a5',
'eaa0a3d415f1a595',
'5765679f0870f4309b1a3c83588024d7c146a4104cf9d2c8',
'11fca73d28d20f8',
'6e9cc7ab82a57f00']

all_permutations = []

for hash in itertools.permutations(hash_parts, 3):
	all_permutations.append(''.join(hash))
 
for hash in all_permutations:
	try:
		if '33C3' in urllib2.urlopen("http://78.46.224.78:5000/payment/callback?data=%s" % hash).read():
			print 'found:',hash
	except:
		pass

# result:
# found: 5765679f0870f4309b1a3c83588024d7c146a4104cf9d2c847aae22e7d77d379272d81aff52de2a54f75c9736d3b8e0641e7995bb92506da1ac7f8da5a628e19ae39825a916d8a2f
# found: 5765679f0870f4309b1a3c83588024d7c146a4104cf9d2c847aae22e7d77d379272d81aff52de2a52f7ef761e2bbe791
# found: 5765679f0870f4309b1a3c83588024d7c146a4104cf9d2c86e9cc7ab82a57f004f75c9736d3b8e0641e7995bb92506da1ac7f8da5a628e19ae39825a916d8a2f
```


&nbsp;

It took the script 2 minutes to run and then it came up with 3 possible hashes, lets try one of them to see if we indeed got the flag:

[<img src="../uploads/pay2win_7.png" />][3]

YES! We got the flag! I took a deep breath and analysed the matched hashes to find out what is the right pattern. I came out with two possible patterns:

<span style="font-size: 10pt;">5765679f0870f4309b1a3c83588024d7c146a4104cf9d2c8 + X + 2f7ef761e2bbe791</span>  
<span style="font-size: 10pt;">5765679f0870f4309b1a3c83588024d7c146a4104cf9d2c8 + X + 4f75c9736d3b8e0641e7995bb92506da1ac7f8da5a628e19ae39825a916d8a2f</span>  
Where X is one of the white parts of &#8216;flag&#8217; product (purchase page/success).

The logic behind is as followed: Seems like the blue part is for &#8216;success&#8217; and the light-blue part is for &#8216;failed&#8217;. The yellow part is likely for &#8216;product page&#8217;. If you take every hash of &#8216;flag&#8217; product (product page / purchase failed) and replace its first part (light-blue / yellow) with the blue part (&#8216;success&#8217;) you come up with a valid hash that brings the flag.

That&#8217;s it. the flag is:  `33C3_3c81d6357a9099a7c091d6c7d71343075e7f8a46d55c593f0ade8f51ac8ae1a8`  
I&#8217;ll be happy to read in the comments how the challenge was for you.

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: http://78.46.224.78:5000/
 [2]: http://www.megabeets.net/uploads/pay2win_6.png
 [3]: http://www.megabeets.net/uploads/pay2win_7.png