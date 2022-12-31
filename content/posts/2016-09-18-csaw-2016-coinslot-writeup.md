---
title: '[CSAW 2016] Coinslot Writeup'
author: Megabeets
type: post
date: 2016-09-18T22:00:33+00:00
url: /csaw-2016-coinslot-writeup/
categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - PPC
  - Writeups

---
#### **Description:**

> _#Hope #Change #Obama2008_
> 
> _nc misc.chal.csaw.io 8000_

Let&#8217;s connect to the server and see what will happen:

<pre class="lang:sh decode:true">[Megabeets] /tmp/CSAW/Coinslot# nc misc.chal.csaw.io 8000
$0.07
$10,000 bills: 0
$5,000 bills: 0
$1,000 bills: 0
$500 bills: 0
$100 bills: 0
...
...</pre>

So, the server is displaying a wanted amount of money and we need to calculate the number of bills and coins given the amount. All we need is writing a simple python script and a coffee break because it will take about 10 minutes for the flag to come up 🙁

<pre class="lang:python decode:true ">from pwn import *

r = remote('misc.chal.csaw.io',8000)

# Create an array of dollars and coins values
money = [10000.0, 5000.0, 1000.0, 500.0, 100.0, 50.0, 20.0, 10.0, 5.0, 1.0, 0.5, 0.25, 0.1, 0.05, 0.01]
count = 0

while(True):
	count += 1
	amount = 0.0
	
	# Recieve the wanted amount of money
	amount = float(r.recvline()[1:])
	print "Wanted amount is " + str(amount)

	# Send the number of dollars and coins for each value
	for m in money:
		print r.recv()
		ans = int(amount/m)
		print "Sending %d" %ans
		r.sendline(str(ans))
		amount = round((amount - (ans*m)), 2)
		print "Left with " + str(amount)
	print "[+] Finished %d" %count
	print r.recvline()</pre>

&nbsp;

The flag is:_ flag{started-from-the-bottom-now-my-whole-team-fucking-here}_

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>