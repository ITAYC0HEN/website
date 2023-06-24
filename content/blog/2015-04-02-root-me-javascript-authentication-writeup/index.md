---
title: 'Private: [Root-Me] Javascript – Authentication Writeup'
author: Megabeets
type: posts
date: 2015-04-02T10:24:52+00:00
categories:
  - Uncategorized
  - Writeups
tags:
  - Javascript
  - Root-Me
  - Writeups

---
Recently I found [this][1] incredible project called **Root-Me** which contains, among other things, over 200 challenges in different levels and topics of the cyber-security field. If you&#8217;re into hacking challenges and wargames I recommend you to check this site out.

I decided to document my solutions for some of the challenges and share it with you, so that beginners can learn and become better hackers. The first challenge we&#8217;re going to solve is called **Javascript &#8211; Authentication. **That&#8217;s the easiest one in the Web-Client category. The challenge, like almost all of the other basic challenges in this category uses a web designing code called HTML (Hyper Text Markup Language) and Javascript along with some others client-side languages. If you know very little or nothing about HTML or Javascript I heartily recommend you read about it either in a book or online.


### **Walkthrough**

When entering the [challenge][2] we&#8217;re presented with a login form which requires a username and a password. The first thing to do is to check what happens when we try to connect with any username and password. I typed test:test, and as I pressed the login button an alert showed up saying: &#8220;wrong password&#8221;.

We know it&#8217;s a client-side challenge which involves JS (the title, duh) so let&#8217;s check out what&#8217;s really happening when we click the login button. To open the browser&#8217;s Inspector, right click on the login button and then select &#8220;Inspect Element&#8221; (you can also press F12, or Ctrl+U if you are old fashioned and prefer the good-old &#8220;View Source&#8221;).

Now we can view the source code of this page. Pressing the login button makes the browser call a Javascript function named &#8220;Login&#8221; (**1**) which exists in external script called &#8220;login.js&#8221;. You can view it under the <script> tag in the <head> section (**2**). Pressing the link shows the code of login.js.

This is the actually code that checks whether the credentials are valid or not. By reading this simple script we understand that the expected username is &#8220;**4dm1n&#8221; **and the password is &#8220;**sh.org&#8221;**. All we have to do now is go back to Root-Me and see if **sh.org **is the correct answer. And voilà, we did it. Easy peasy.





 [1]: http://root-me.org/
 [2]: https://www.root-me.org/en/Challenges/Web-Client/Javascaript-Authentication