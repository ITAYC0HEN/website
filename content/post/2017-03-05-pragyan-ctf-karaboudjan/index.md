---
title: '[Pragyan CTF] The Karaboudjan'
author: Megabeets
type: post
date: 2017-03-05T07:32:09+00:00
url: /pragyan-ctf-karaboudjan/
categories:
  - Pragyan CTF 2017
  - Writeups
tags:
  - CTF
  - Forensics
  - Writeups

---
## Description

> **The Karaboudjan | **Forensics 150 pts
> 
> Captain Haddock is on one of his ship sailing journeys when he gets stranded off the coast of North Korea. He finds shelter off a used nuke and decides to use the seashells to engrave a message on a piece of paper. Decrypt the message and save Captain Haddock.
> 
> ->-.>-.&#8212;.&#8211;>-.>.>+.&#8211;>&#8211;..++++.
> 
> <hr class="bbcode_rule" />
> 
> .+++.
> 
> <hr class="bbcode_rule" />
> 
> .->-.->-.++++++++++.+>+++.++.-[->+++<]>+.+++++.++++++++++..++++[->+++<]>.&#8211;.->&#8211;.>.
> 
> <a class="has-tooltip" title="" href="https://ctf.pragyan.org/download?file_key=c279a3923f124ea36dc67a930a55c996ae14b0661a86f7e61a2db0690d97bd4e&team_key=a500afc4a171f394f280518fefd78d62f976bf8303f77f3431573fce01c983cb" data-toggle="tooltip" data-placement="right" data-original-title="0.28 KB">clue.zip</a>

&nbsp;

This was funny challenge, I struggled with that Brainfuck but all it was is just brainfuck. Nothing more, we don&#8217;t need it to solve the challenge. Sorry guys.

I downloaded the zip file which was encrypted, I then cracked it using &#8220;fcrakzip&#8221; and dictionary attack. And found that the password is &#8220;_dissect_&#8220;. Inside the zip was a pcap file with one packet:

&nbsp;

<img data-attachment-id="987" data-permalink="https://www.megabeets.net/pragyan-ctf-karaboudjan/krabu1/#main" data-orig-file="http://www.megabeets.net/uploads/krabu1.png" data-orig-size="735,178" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="krabu1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/krabu1-300x73.png" data-large-file="http://www.megabeets.net/uploads/krabu1.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-987" src="https://www.megabeets.net/uploads/krabu1.png" alt="" width="735" height="178" srcset="https://www.megabeets.net/uploads/krabu1.png 735w, https://www.megabeets.net/uploads/krabu1-150x36.png 150w, https://www.megabeets.net/uploads/krabu1-300x73.png 300w" sizes="(max-width: 735px) 100vw, 735px" /> 

That&#8217;s it, we got the flag 🙂

The flag was **pragyanctf{5n00p_d099}**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>