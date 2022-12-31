---
title: 'Flare-On5 Writeup – Challenge 1: Minesweeper Championship Registration'
author: Megabeets
type: post
date: -001-11-30T00:00:00+00:00
draft: true
url: /?p=1563
categories:
  - Uncategorized

---
## blah blah

blah blah

&nbsp;

&nbsp;

> ## Minesweeper Championship Registration {.chal-name.text-center.pt-3}
> 
> Welcome to the Fifth Annual Flare-On Challenge! The Minesweeper World Championship is coming soon and we found the registration app. You weren&#8217;t _officially_invited but if you can figure out what the code is you can probably get in anyway. Good luck!
> 
> 7zip Password: infected
> 
> Download:

&nbsp;

&nbsp;

We are given with a JAR file named &#8220;MinesweeperChampionshipRegistration.jar&#8221;. Just to be sure it is really a JAR file we can use the `file` command:

$ file MinesweeperChampionshipRegistration.jar  
MinesweeperChampionshipRegistration.jar: Java archive data (JAR)

So it&#8217;s indeed a JAR file. Since it&#8217;s the first challenge, I assumed the flag would simply be in the strings of the Class file inside the JAR. And indeed, this simple oneliner gave the flag:

$unzip -c MinesweeperChampionshipRegistration.jar | strings | grep @flare  
GoldenTicket2018@flare-on.com

But just for the fun, we can use radare2 to list the files inside the JAR and then interpret the relevant Class file.

$ r2 zip://MinesweeperChampionshipRegistration.jar  
0 META-INF/MANIFEST.MF  
1 InviteValidator.class

So the class we need to reverse is &#8220;InviteValidator.class&#8221;. Let&#8217;s open it in radare2:

$ r2 zip://MinesweeperChampionshipRegistration.jar//InviteValidator.class  
[0x0000045f]>

Now that we are in radare2, and since it is the first challenge, we can simply use `iz` to print the strings in the binary and then grep for &#8220;flare&#8221; using `~` which is radare&#8217;s internal grep:

[0x0000045f]> izq~flare  
0x187 32 29 GoldenTicket2018@flare-on.com\

&nbsp;

Or a oneliner with `rabin2`:

$ rabin2 -zq zip://MinesweeperChampionshipRegistration.jar//InviteValidator.class | grep @flare  
GoldenTicket2018@flare-on.com

&nbsp;

And the flag is: `GoldenTicket2018@flare-on.com`

&nbsp;

&nbsp;

&nbsp;

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>