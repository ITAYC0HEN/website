---
title: '[H4CK1T 2016] v01c3_0f_7h3_fu7ur3 – Australia Writeup'
author: Megabeets
type: post
date: 2016-10-02T23:02:06+00:00
url: /h4ck1t-2016-v01c3_0f_7h3_fu7ur3-australia-writeup/
categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - Forensics
  - H4CK1T
  - Writeups

---
### **Description:**

> **v01c3\_0f\_7h3_fu7ur3 &#8211; Australia &#8211; 300 &#8211; Network**  
> <span style="font-weight: 400;">The captured data contains encrypted information. Decrypt it.</span>  
> <span style="font-weight: 400;"><a href="http://ctf.com.ua/data/attachments/wireshark_8764d640d217fd346e2db2b5c38dde13.pcap">http://ctf.com.ua/data/attachments/wireshark_8764d640d217fd346e2db2b5c38dde13.pcap</a></span>

The first thing I do when I face a pcap challenge is, of course, open it in Wireshark. If it looks normal (and not, for example, [Bluetooth traffic][1]) I then run &#8216;_foremost_&#8216; on the file. &#8216;_foremost_&#8216; is searching for a known files in a given file by file headers, footers etc, and then extract it to &#8216;output&#8217; folder in the directory.  
So foremost found several files in the PCAP from several sources like http and ftp traffic

<ul style="list-style-type: square;">
  <li>
    png
  </li>
  <li>
    gif
  </li>
  <li>
    jpg
  </li>
  <li>
    rar
  </li>
  <li>
    (&#8230;)
  </li>
</ul>

I opened the rar archive and found a file named &#8216;key.enc&#8217; which contained &#8220;Salted_<GIBBERISH>&#8221; . I opened it in hex editor:

<img src="../uploads/h4ck1t_australia_1.png" /> 

At the first, as the name says, I thought I found the key of some encryption and now I need to find the encrypted file and the cipher. But in a second thought I said to myself that &#8216;*.enc&#8217; is usually for the encrypted files! So that file isn&#8217;t a key, it&#8217;s encrypted and we need to decrypt it. But what is the key and the cipher?

So, I figured out that file that starting with &#8220;Salted_&#8221; is file that was encrypted using &#8216;openssl&#8217; application.  
I then went to read the task again, I saw that the name of the challenge is &#8220;v01c3\_0f\_7h3_fu7ur3&#8221; so I thought maybe it involves some audio. Searched for &#8216;mp3&#8217; or &#8216;aud&#8217; in the pcap (queries: &#8216;tcp contains mp3&#8217; , &#8216;tcp contains aud&#8217;) and found the following url:  
<a href="http://priyom.org/scripts/audioplayer.min.js" target="_blank">http://priyom.org/scripts/audioplayer.min.js</a>

It&#8217;s an innocent javascript file. I entered the &#8220;priyom&#8221; site and read it&#8217;s description:

> &#8220;Priyom is an international organization intending to research and bring to light the mysterious reality of intelligence, military and diplomatic communication via shortwave radio: number stations&#8221;

Sounds interesting. So I looked up again in the pcap and saw a request to this specific url:  
<a href="http://priyom.org/number-stations/english/e06" target="_blank">http://priyom.org/number-stations/english/e06</a>

There is a robotic voice that reads out numbers.  
75975975948648631317369873698599905999017212172126397363973486486313100000

So I now have what seems like a key, so what is the encryption?  
A bit research about the encryption made me think it&#8217;s AES so I ran:

```sh
openssl aes-256-cbc -d -in key.enc -k &lt;the long key&gt;
```


-d is for decrypt  
-k is for keyphrase

Failed. So I read about the structure of the voice record in the website and took only the Message part from the numbers: 7369859990172126397300000  
this is the actual Message part (5-digit paired groups) and 5 zeroes at the end. without the Intro, Outro, Premable, Postmable and the Duplicate 5-digits.

Failed again. Tried it with all the possible _openssl_  encryptions (20+) but failed again.  
So I got mad and tried to decrypt it using all possible encryptions with all possible substrings of the original number from the record.  
Pseudo code:

```python
for sb in all_possible_substrings(key)
{
	for enc in all_possible_encryptions:
	(
		openssl encr -d -in key.enc -k sb
	)
}
```


And how it was really looks like:

<img src="../uploads/h4ck1t_australia_2.jpg" />  
it took 30 minutes to run.  
BUT FAILED. No flag.

At this point I think that 3 or 4 teams already solved it.  
So I tried more and more combinations and this stupid one finally worked:

```sh
Megabeets:/tmp/h4ckit/australia# openssl aes-256-cbc -d -in key.enc -k 75948631736985999017212639734863100000
h4ck1t{Nic3_7ry}
```


It&#8217;s the full number from the recording but delete the duplicates pairs (the recording was splitted to group of numbers and the speaker said each group twice or three times).

So the hardest part was actually to figure out the exact keyphrase, the rest was pretty easy.

**Flag:**_ h4ck1t{Nic3_7ry}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: http://www.megabeets.net/asis-ctf-skyblue/