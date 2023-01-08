---
title: 'Private: [Root-Me] Javascript – Source Writeup'
author: Megabeets
type: post
date: 2015-04-03T19:10:00+00:00
draft: true
private: true
url: /?p=131
categories:
  - Uncategorized
  - Writeups
tags:
  - Javascript
  - Root-Me
  - Writeups

---
Continuing from my previous post I&#8217;d started tinkering around with the next Root-Me challenge, and this time it was even easier than the previous one.

**Video**

** **

**Walkthrough**

As we enter the [challenge][1] we&#8217;re faced with a prompt form asks us to enter a password. Wrong passwords entered result with an alert showing up saying: &#8220;wrong password !&#8221;. Let&#8217;s close this alert and check if anything hides in the source code (Ctrl+U). A short view reveals us that the <body> element uses an event called &#8220;onload&#8221;, which calls a &#8220;login&#8221; function. According to <a href="http://www.w3schools.com/" target="_blank">w3schools</a> the &#8220;onload&#8221; event is most commonly used within the <body> element to execute a script once a web page has completely loaded all of its content (Images, script files, CSS files, etc.).

The function simply compares the password we&#8217;ve entered with a hard coded clear-text password.  
The password is: &#8220;**123456azerty**&#8220;.  
You can use this password to validate the challenge.

<p style="text-align: right;">
  <img src="./megabeets_inline_logo.png" />Eat Veggies.
</p>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="./megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://www.root-me.org/en/Challenges/Web-Client/Javascript-Source