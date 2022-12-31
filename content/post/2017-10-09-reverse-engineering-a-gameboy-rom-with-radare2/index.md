---
title: Reverse engineering a Gameboy ROM with radare2
author: Megabeets
type: post
date: 2017-10-09T18:32:50+00:00
url: /reverse-engineering-a-gameboy-rom-with-radare2/
categories:
  - Articles
  - radare2
  - Writeups
tags:
  - CTF
  - gameboy
  - radare2
  - tutorial
  - Writeups

---
## <span class="ez-toc-section" id="_Prologue"></span> Prologue<span class="ez-toc-section-end"></span>

A month ago in Barcelona I was attending to [r2con][1] for the first time. This is the official congress of the radare2 community where everyone can learn more about radare2 framework and dive deep into different aspects of reverse engineering, malware analysis, fuzzing, exploiting and more. It also the place where all of us, the contributors and developers of radare2, can meet, discuss and argue about every other line of code in the framework.

This was the second congress of radare2, after the success of the first congress in last year which was also a celebration for radare&#8217;s 10 years old birthday. This year the conference was bigger, fancier and probably organized much better. r2con was four days long, starting at September 6 and lasted until September 9. The first two days were dedicated to training and took place at _Universitat de Barcelona._ The other two days were talks days and took place at the _MediaPro._

## <span class="ez-toc-section" id="Crackmes_Competition"></span>Crackmes Competition<span class="ez-toc-section-end"></span>

During r2con this year there was a Crackmes competition where all the attendees were given with the same 5 challenges and had to publish a writeups to all the challenges they had solved. The scoring was based on the quality of the writeups along with the quantity of solved challenges.

<p style="text-align: center;">
  <span style="font-size: 14pt;"><strong>I won the competition and got myself some cool swag!</strong></span>
</p>

  * Flag of radare2
  * [POC | GTFO][2] book
  * Orange PI with 3D printed case of r2con logo
  * Radare2 stickers
  * A beer 🍺

[<img src="../uploads/r2con17_swags.png" />][3]

I thought of sharing some of my writeups with you, so you can taste a bit from what we had in the competition and so that others, coming from google, twitter and such, could learn how to use radare2 for solving different challenges. This article is aimed to those of you who are familiar with radare2. If you are not, I suggest you to start from [part 1][4] of my series _&#8220;A Journy Into Radare2&#8221;._

## <span class="ez-toc-section" id="Getting_radare2"></span>Getting radare2<span class="ez-toc-section-end"></span>

### <span class="ez-toc-section" id="Installation"></span>Installation<span class="ez-toc-section-end"></span>

Radare2’s development is pretty quick – the project evolves every day, therefore it&#8217;s recommended to use the current git version over the stable one. Sometimes the stable version is less stable than the current git version!

```sh
$ git clone https://github.com/radare/radare2.git
$ cd radare2
$ ./sys/install.sh
```


If you don&#8217;t want to install the git version or you want the binaries for another machine (Windows, OS X, iOS, etc) check out the [download page at the radare2 website.][5]

### <span class="ez-toc-section" id="Updating"></span>Updating<span class="ez-toc-section-end"></span>

As I said before, it is highly recommended to always use the newest version of r2 from the git repository. All you need to do to update your r2 version from the git is to execute:

```sh
$ ./sys/install.sh
```


And you&#8217;ll have the latest version from git. I usually update my version of radare2 in the morning, while watching cat videos.

## 

<blockquote class="twitter-tweet" data-lang="en">
  <p dir="ltr" lang="en">
    Let&#8217;s use radare2 to reverse engineer a Gameboy ROM!<br /> Check it out @ <a href="https://t.co/g1kYJuShzE">https://t.co/g1kYJuShzE</a><a href="https://twitter.com/radareorg?ref_src=twsrc%5Etfw">@radareorg</a> <a href="https://twitter.com/hashtag/radare2?src=hash&ref_src=twsrc%5Etfw">#radare2</a> <a href="https://t.co/7FJOB3SdKS">pic.twitter.com/7FJOB3SdKS</a>
  </p>
  
  <p>
    — Itay Cohen (@megabeets_) <a href="https://twitter.com/megabeets_/status/917464989027454976?ref_src=twsrc%5Etfw">October 9, 2017</a>
  </p>
</blockquote>



## <span class="ez-toc-section" id="Playing_with_Gameboy_ROM"></span>Playing with Gameboy ROM<span class="ez-toc-section-end"></span>

This post will describe how I solved [_simple.gb_][6], a Gameboy ROM challenge written by _@condret_. It was actually my first time reversing a Gameboy ROM &#8212; and it was awesome!

First thing I did was to open the binary in radare2 and check for its architecture and format:

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="font-weight: 400;"><span style="color: #ffff99;">$</span> r2 simple.gb</span><br /> &#8212; For a full list of commands see `strings /dev/urandom`<br /> <span style="color: #468ee6;">[<span style="font-weight: 400;">0x00000100</span>]></span> i~format<br /> <span style="font-weight: 400;">format   ningb</span><br /> <span style="color: #468ee6;">[0x00000100]></span> i~machine<br /> <span style="font-weight: 400;">machine  </span><span style="font-weight: 400; color: #00ccff;">Gameboy</span>
      </td>
    </tr>
  </table>
</div>

> <span style="font-weight: 400;">The <code>i</code> command gives us </span>**i**<span style="font-weight: 400;">nformation about the binary. Check <code>i?</code> for more commands.</span>
> 
> Tilde (`~`) is r2&#8217;s internal grep.

Surprise, surprise, it is a Gameboy ROM &#8212; dah. <span style="font-weight: 400;">After reading a bit about its </span>[<span style="font-weight: 400;">instruction set</span>][7] <span style="font-weight: 400;">we should go to the mission. </span>

The obvious thing to do is open the ROM in an Gameboy emulator. <span style="font-weight: 400;">I downloaded the good old emulator I used back in the days when I played Pokemon: </span>[<span style="font-weight: 400;">VisualBoy Advance</span>][8]<span style="font-weight: 400;">.</span>

<span style="font-weight: 400;">Let&#8217;s open the ROM in our emulator and see what we have:</span>

[<img src="../uploads/pokemonGold_VGA.png" />][9]

<span style="font-weight: 400;"><br /> Woops, wrong file. Bad habits&#8230; Let&#8217;s try again:</span>

[<img src="../uploads/simple_gb_screenshot1.png" />][10]

<span style="font-weight: 400;">Cool! It&#8217;s a simple game where, by using the arrow keys, you increase/decrease 5 digits. We &#8216;simply&#8217; need to find the correct password.</span>

<!--more-->

<span style="font-weight: 400;">After randomly choosing digits and pressing the Enter key we receive the </span><i style="font-family: inherit; font-weight: inherit;"><span style="font-weight: 400;">badboy </span></i><span style="font-weight: 400;">message:</span>

[<img src="../uploads/simple_gb_screenshot2.png" />][11]

<span style="font-weight: 400;">Okay let&#8217;s start analyzing the code and search for the function that checks our input:</span>

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="color: #468ee6;">[<span style="font-weight: 400;">0x00000100</span>]></span> aaa<br /> <span style="color: #35d7c7;">[x]</span> Analyze all flags starting with sym. and entry0 (aa)<br /> <span style="color: #35d7c7;">[x]</span> Analyze len bytes of instructions for references (aar)<br /> <span style="color: #35d7c7;">[x]</span> Analyze function calls (aac)<br /> <span style="color: #35d7c7;">[x]</span> Use -AA or aaaa to perform additional experimental analysis.<br /> <span style="color: #35d7c7;">[x]</span> Constructing a function name for fcn.* and sym.func.* functions (aan)
      </td>
    </tr>
  </table>
</div>

<span style="font-weight: 400;">Usually my plan in such cases is to start from the end, i.e from where the &#8220;<em>FAIL!&#8221;</em> message is printed. That’s because the checking function should probably be near by.</span>

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x00000100</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> izzq</span><span style="font-weight: 400;">~</span><span style="font-weight: 400;">FAIL</span><br /> <span style="font-weight: 400;">0x2f3</span> <span style="font-weight: 400;">6</span> <span style="font-weight: 400;">5</span><span style="font-weight: 400;"> FAIL!</span>
      </td>
    </tr>
  </table>
</div>

> <span style="font-weight: 400;"><code>izzq</code> would print us strings that exist in the whole binary, <code>q</code> is for quieter output.</span>

<span style="font-weight: 400;">Cool, now that we know the address of </span><span style="font-weight: 400;">&#8220;</span>_<span style="font-weight: 400;">FAIL</span>__<span style="font-weight: 400;">!</span>_<span style="font-weight: 400;">&#8220;</span> <span style="font-weight: 400;">let&#8217;s find where it referenced from. <em>S</em></span>_<span style="font-weight: 400;">adly xrefs doesn&#8217;t work this time so I did it the ugly way &#8212; grepping.</span>_

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x00000100</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> pd </span><span style="font-weight: 400;">1024</span><span style="font-weight: 400;">~</span><span style="font-weight: 400;">2f3</span><br /> <span style="font-weight: 400;">0x000002e4      21f302         ld hl, 0x02f3</span>
      </td>
    </tr>
  </table>
</div>

> <span style="font-weight: 400;"><code>pd</code> stands for </span>**p**<span style="font-weight: 400;">rint </span>**d**<span style="font-weight: 400;">isassembly. Check <code>pd?</code> for more commands.</span>

<span style="font-weight: 400;">We found that it referenced at </span>**0x2e4** <span style="font-weight: 400;">so let&#8217;s seek to this address and print the function:</span>

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x00000100</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> s </span><span style="font-weight: 400;">0x2e4</span><br /> <span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x000002e4</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> pdf</span><br /> <span style="font-weight: 400;">┌</span> <span style="font-weight: 400;">(</span><span style="font-weight: 400;">fcn</span><span style="font-weight: 400;">)</span><span style="font-weight: 400;"> fcn</span><span style="font-weight: 400;">.</span><span style="font-weight: 400;">00000274</span> <span style="font-weight: 400;">107</span><br /> <span style="font-weight: 400;">│</span><span style="font-weight: 400;">   fcn</span><span style="font-weight: 400;">.</span><span style="font-weight: 400;">00000274</span> <span style="font-weight: 400;">();</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x00000274</span><span style="font-weight: 400;">      f802           ld hl</span><span style="font-weight: 400;">,</span><span style="font-weight: 400;"> sp </span><span style="font-weight: 400;">+</span> <span style="font-weight: 400;">0x02</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x00000276</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">5e</span><span style="font-weight: 400;">             ld e</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">[</span><span style="font-weight: 400;">hl]</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x00000277</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">23</span><span style="font-weight: 400;">             inc hl</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x00000278</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">56</span><span style="font-weight: 400;">             ld d</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">[</span><span style="font-weight: 400;">hl]</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x00000279</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">210400</span><span style="font-weight: 400;">         ld hl</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">0x0004</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x0000027c</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">19</span><span style="font-weight: 400;">             add hl</span><span style="font-weight: 400;">,</span><span style="font-weight: 400;"> de</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x0000027d</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">4d</span><span style="font-weight: 400;">             ld c</span><span style="font-weight: 400;">,</span><span style="font-weight: 400;"> l</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x0000027e</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">44</span><span style="font-weight: 400;">             ld b</span><span style="font-weight: 400;">,</span><span style="font-weight: 400;"> h</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x0000027f</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">0a</span><span style="font-weight: 400;">             ld a</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">[</span><span style="font-weight: 400;"><span style="font-weight: 400;">bc]  </span></span><b style="font-family: inherit; font-size: 13.3333px; font-style: inherit;"><.. truncated ..></b><span style="font-weight: 400;">│</span> <b>0x000002bd</b><b>      fe01           cp </b><b>0x01</b> <b>;</b> <b><i>comparison</i></b><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002bf</span><span style="font-weight: 400;">      c2e402         jp nZ</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">0x02e4</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002d3</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">1803</span><span style="font-weight: 400;">           jr </span><span style="font-weight: 400;">0x03</span><br /> <b>| 0x000002d5</b><b>      c3e402         jp </b><b>0x02e4</b><b>          </b><b>;</b> <b><i>jump to badboy</i></b><br /> <span style="font-weight: 400;">│</span><span style="font-weight: 400;">    </span><span style="font-weight: 400;">;</span><span style="font-weight: 400;"> JMP XREF </span><span style="font-weight: 400;">from</span> <span style="font-weight: 400;">0x000002d3</span> <span style="font-weight: 400;">(</span><span style="font-weight: 400;">fcn</span><span style="font-weight: 400;">.</span><span style="font-weight: 400;">00000274)</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002d8</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">21ee02</span><span style="font-weight: 400;">         ld hl</span><span style="font-weight: 400;">,</span> <b>0x02ee</b> <b>;</b> <b><i>another string</i></b><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002db</span><span style="font-weight: 400;">      e5             push hl</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002dc</span><span style="font-weight: 400;">      cd660f         call fcn</span><span style="font-weight: 400;">.</span><span style="font-weight: 400;">00000f66</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002df</span><span style="font-weight: 400;">      e802           add sp</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">0x02</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002e1</span><span style="font-weight: 400;">      c3ed02         jp </span><span style="font-weight: 400;">0x02ed</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002e4</span><span style="font-weight: 400;">      </span><span style="font-weight: 400;">21f302</span><span style="font-weight: 400;">         ld hl</span><span style="font-weight: 400;">,</span> <b>0x02f3</b> <b>;</b> <b><i>our badboy</i></b><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002e7</span><span style="font-weight: 400;">      e5             push hl</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002e8</span><span style="font-weight: 400;">      cd660f         call fcn</span><span style="font-weight: 400;">.</span><span style="font-weight: 400;">00000f66</span><br /> <span style="font-weight: 400;">│</span> <span style="font-weight: 400;">0x000002eb</span><span style="font-weight: 400;">      e802           add sp</span><span style="font-weight: 400;">,</span> <span style="font-weight: 400;">0x02</span><br /> <span style="font-weight: 400;">│</span><span style="font-weight: 400;">    </span><span style="font-weight: 400;">;</span><span style="font-weight: 400;"> JMP XREF </span><span style="font-weight: 400;">from</span> <span style="font-weight: 400;">0x000002e1</span> <span style="font-weight: 400;">(</span><span style="font-weight: 400;">fcn</span><span style="font-weight: 400;">.</span><span style="font-weight: 400;">00000274)</span><br /> <span style="font-weight: 400;">└</span> <span style="font-weight: 400;">0x000002ed</span><span style="font-weight: 400;">      c9             ret</span>
      </td>
    </tr>
  </table>
</div>

> <span style="font-weight: 400;"><code>s</code> is used to </span>**s**<span style="font-weight: 400;">eek to address and <code>pdf</code> stands for </span>**p**<span style="font-weight: 400;">rint </span>**d**<span style="font-weight: 400;">isassembly </span>**f**<span style="font-weight: 400;">unction</span>

radare recognized that our function begins at _0x274_. We can see at the bottom some comparison, then it jumps to our _badboy_ message or to another message which is a string at _0x02ee_. Let&#8217;s see what string is hiding there:

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x000002e4</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> ps @ 0x02ee</span><br /> <span style="font-weight: 400;">WIN!</span>
      </td>
    </tr>
  </table>
</div>

> `ps` stands for **p**rint **s**tring. `@` is a temporary seek.

<span style="font-weight: 400;">Great! We found our </span>_<span style="font-weight: 400;">goodboy</span>_<span style="font-weight: 400;">!<br /> </span><span style="font-weight: 400;">Let&#8217;s rename the function to </span>`<span style="font-weight: 400;">check_input</span>`<span style="font-weight: 400;"> and start analyze it:</span>

<div style="overflow-x: auto; margin: 0 0 20px;">
  <table style="background-color: #4a3446; color: #fff; font-family: terminal, monaco, monospace; font-size: 10pt;">
    <tr style="height: 19.8594px;">
      <td style="height: 19.8594px;">
        <span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x000002e4</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> afn check_input </span><span style="font-weight: 400;">0x274<br /> </span><span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x00000274</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> s check_input<br /> </span><span style="color: #468ee6;"><span style="font-weight: 400;">[</span><span style="font-weight: 400;">0x00000274</span><span style="font-weight: 400;">]></span></span><span style="font-weight: 400;"> VV</span>
      </td>
    </tr>
  </table>
</div>

> <span style="font-weight: 400;">The <code>afn</code> command stands for  </span>**a**<span style="font-weight: 400;">nalyze </span>**f**<span style="font-weight: 400;">unction <strong>n</strong></span><span style="font-weight: 400;">ame. <code>VV</code> will open the Visual Graph Mode</span>

<span style="font-weight: 400;">I used Visual Graph mode for convenience, the function combined from a lot of <em>jumps</em> and <em>if-conditions</em> so it&#8217;s much more easier that way.</span>

[<img src="../uploads/conditions.png" />][12]

> Use `p` and `P` to toggle between the different visual modes.

<span style="font-weight: 400;">Seems like we found the place where the function checks each digit and compares it with the correct one. On the left we can see the flow of valid digits. Let&#8217;s have a quick view of the blocks. We&#8217;ll toggle again between the views using <code>p</code> until we reach the regular graph mode.</span>

<p style="text-align: center;">
  <a href="https://www.megabeets.net/uploads/firstlook.png"><img src="../uploads/firstlook.png" /></a>
</p>

<p style="text-align: center;">
  <em>This is just a snip of the full graph since it is too long to put as an image.</em>
</p>

<span style="font-weight: 400;">From a quick view, and without fully understanding the instructions, it seems like the binary is checking whether the digit in each place is equals to a specific immediate value. It checks it by using <code>cmp &lt;em>imm&lt;/em></code></span><span style="font-weight: 400;"> in this order: 3, 7, 5, 1, 9.<br /> Hooray! That was so easy, let&#8217;s just enter this value and get the <em>WIN!</em> message:</span>

[<img src="../uploads/simple_gb_screenshot3.png" />][13]

<span style="font-weight: 400;"><br /> What? Fail?? Okay, okay, maybe the other way:</span>

[<img src="../uploads/simple_gb_screenshot4.png" />][14]

<span style="font-weight: 400;">Bummer&#8230; So it&#8217;s not so easy after all. I probably need to have a closer look at this function.</span>

<span style="font-weight: 400;">After a while and a deeper understanding of the function and each condition (</span>_<span style="font-weight: 400;">radare2 isn&#8217;t able to debug the program so it was even harder</span>_<span style="font-weight: 400;">) I understood that the blocks are not so different from each other, except several lines. Although it&#8217;s a simple lines of code, the disassembly looked awful. the register <code>bc</code> is holding the address of our array and being manipulated during the execution of the program. All we had to do is to follow its changes and understand which digit is being checked in every block.</span>

Let&#8217;s take a look on these blocks for example:

```diff
[0x000002e4]&gt; pdf                                                                          
/ (fcn) fcn.00000274 107                                                                   
|   fcn.00000274 ();                                                                       
|           0x00000274      f802           ld hl, sp + 0x02                                
|           0x00000276      5e             ld e, [hl]                                      
|           0x00000277      23             inc hl                                          
|           0x00000278      56             ld d, [hl]                                      
|           0x00000279      210400         ld hl, 0x0004                                   
|           0x0000027c      19             add hl, de                                      
|           0x0000027d      4d             ld c, l                                         
|           0x0000027e      44             ld b, h                                         
|           0x0000027f      0a             ld a, [bc]                                      
|           0x00000280      4f             ld c, a                                         
|           0x00000281      fe03           cp 0x03                                         
|       ,=&lt; 0x00000283      c2e402         jp nZ, 0x02e4                                   
|      ,==&lt; 0x00000286      1803           jr 0x03                                         
      ,===&lt; 0x00000288      c3e402         jp 0x02e4                   ; fcn.00000274+0x70 
|     |||      ; JMP XREF from 0x00000286 (fcn.00000274)                                   
|     |`--&gt; 0x0000028b      f802           ld hl, sp + 0x02                                
|     | |   0x0000028d      4e             ld c, [hl]                                      
|     | |   0x0000028e      23             inc hl                                          
|     | |   0x0000028f      46             ld b, [hl]                                      
|     | |   0x00000290      03             inc bc                                          
|     | |   0x00000291      03             inc bc                                          
|     | |   0x00000292      0a             ld a, [bc]                                      
|     | |   0x00000293      4f             ld c, a                                         
|     | |   0x00000294      fe07           cp 0x07                                         
|     |,==&lt; 0x00000296      c2e402         jp nZ, 0x02e4                                   
|    ,====&lt; 0x00000299      1803           jr 0x03                                         
    ,=====&lt; 0x0000029b      c3e402         jp 0x02e4                   ; fcn.00000274+0x70 
|   |||||      ; JMP XREF from 0x00000299 (fcn.00000274)                                   
|   |`----&gt; 0x0000029e      f802           ld hl, sp + 0x02                                
|   | |||   0x000002a0      4e             ld c, [hl]                                      
|   | |||   0x000002a1      23             inc hl                                          
|   | |||   0x000002a2      46             ld b, [hl]                                      
|   | |||   0x000002a3      03             inc bc                                          
|   | |||   0x000002a4      0a             ld a, [bc]                                      
|   | |||   0x000002a5      4f             ld c, a                                         
|   | |||   0x000002a6      fe05           cp 0x05                                         
|   |,====&lt; 0x000002a8      c2e402         jp nZ, 0x02e4                                   
|  ,======&lt; 0x000002ab      1803           jr 0x03                                         
  ,=======&lt; 0x000002ad      c3e402         jp 0x02e4                   ; fcn.00000274+0x70
```


At the first block, _0x4_ is moved (`ld` instruction) to `hl` which in turn moved to register `bc` and then the value which is referenced in `bc` is compared to _0x3_. As I said, `bc` points to our input so this check checks whether `bc+4` is equals to _0x4_. In the next block we can see that `bc`, which returned to its original value, now increased (`inc`) twice and the value it is referenced to is compared with _0x7_. In the last block of our example, `bc` returns to its initial value and then incremented once and its referenced value is compared with _0x5_..

<span style="font-weight: 400;">Finally I came up with this pseudo Pythonic code:</span>

```python
def check_password (guess):
    if guess[4]==3 and guess[2]==7 and guess[1]==5 and guess[3]==1 and guess[0]==9:
        print "WIN!"
    else:
        print "FAIL!"
```


Let’s see if we got it right:

[<img src="../uploads/simple_gb_screenshot5-1.png" />][15]

<span style="font-weight: 400;">Yes! We did it and ended up with our beloved </span>_<span style="font-weight: 400;">goodboy.</span>_

## <span class="ez-toc-section" id="Epilogue"></span>Epilogue<span class="ez-toc-section-end"></span>

In this writeup I showed you more of the powers within radare2, this time its capabilities to analyze a non-trivial binary. <span style="font-weight: 400;">Thanks </span>_<span style="font-weight: 400;">condret</span>_ <span style="font-weight: 400;">and all of the great people in the </span>_<span style="font-weight: 400;">radare </span>_<span style="font-weight: 400;">community </span><span style="font-weight: 400;">for this great challenge! Another thanks goes to all these angels who helped creating r2con this year, it was absolutely awesome.</span>

If you want to learn more about radare2 I suggest you to start from the [part 1][4] of my series _&#8220;A Journy Into Radare2&#8221;._

As always, please post comments to this post or message me [privately][16] if something is wrong, not accurate, needs further explanation or you simply don’t get it. Don’t hesitate to share your thoughts with me

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: http://rada.re/con/2017/
 [2]: https://www.amazon.com/PoC-GTFO-MANUL-Laphroaig/dp/1593278802
 [3]: https://www.megabeets.net/uploads/r2con17_swags.png
 [4]: https://www.megabeets.net/a-journey-into-radare-2-part-1/
 [5]: http://radare.org/r/down.html
 [6]: https://github.com/ITAYC0HEN/A-journey-into-Radare2/blob/master/Generic/Reversing%20Gameboy%20ROM/simple.gb
 [7]: http://marc.rawer.de/Gameboy/Docs/GBCPUman.pdf
 [8]: https://sourceforge.net/projects/vba
 [9]: https://www.megabeets.net/uploads/pokemonGold_VGA.png
 [10]: https://www.megabeets.net/uploads/simple_gb_screenshot1.png
 [11]: https://www.megabeets.net/uploads/simple_gb_screenshot2.png
 [12]: https://www.megabeets.net/uploads/conditions.png
 [13]: https://www.megabeets.net/uploads/simple_gb_screenshot3.png
 [14]: https://www.megabeets.net/uploads/simple_gb_screenshot4.png
 [15]: https://www.megabeets.net/uploads/simple_gb_screenshot5-1.png
 [16]: https://www.megabeets.net/about.html#contact