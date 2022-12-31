---
title: '[H4CK1T 2016] PhParanoid – Malaysia Writeup'
author: Megabeets
type: post
date: 2016-10-02T20:35:48+00:00
url: /h4ck1t-2016-phparanoid-malaysia/
categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - Reverse
  - Writeups

---
&nbsp;

### **Description**:

> **Task: PhParanoid &#8211; Malaysia &#8211; 225 &#8211; Rever$e**
> 
> <span style="font-weight: 400;">EN: I am so paranoid! I try to hide everything from this mad world! I have already obfuscated my calculator sources, my javascript site sources and I`m not going to stop! And u will never know what I hide, haha!</span>

In this challenge we got _Phb_, i.e php file that compiled using BCompiler (PHP Bytecode Compiler). We can Decompile it using [this][1].

The decompilation process is very simple and we easily got this php file (I deleted repeated parts to decrease size):

```php
<?php
$secret ="The";
do {
	$is_secret_exists = false;

	if (isset($secret)) {
		$is_secret_exists = true;
	}
	else {
		break;
	}

	$is_secret_valid = false;
	if (strstr($secret, "The") && (strpos($secret, "The") == 0)) {
		$is_secret_valid = true;
		echo $secret;
	}

	$c0 = chr(ord($secret[0]) + 20);

	if ((ord($c0) + (-20)) != 84) {
		unset($c0);
		break;
	}

	$c1 = chr(ord($secret[1]) + (-52));

	if ((ord($c1) + 52) != 104) {
		unset($c1);
		break;
	}
	
		...
		...
		...

	$c43 = chr(ord($secret[43]) + (-35));

	if ((ord($c43) + 35) != 103) {
		unset($c43);
		break;
	}

	$c44 = chr(ord($secret[44]) + 79);

	if ((ord($c44) + (-79)) != 46) {
		unset($c44);
		break;
	}
} while (false);

?>

```


&nbsp;

So what we have here is a manipulation on char codes to check if the _secret _variable is valid. Let&#8217;s find out which char codes we need to use in order to find the flag.

Every if statement in the code looks like the following code. I added a comments for explanation

```php
# the first character of the flag is the first character of the 'secret' + 20
$c0 = chr(ord($secret[0]) + 20);

# check if the char code of the first character of the flag -20 equals 84
if ((ord($c0) + (-20)) != 84) {
	unset($c0);
	break;
}
```


So, from this we can assume that the flag starts with chr(20+84) which is &#8216;h&#8217;. Make sense because we know that the flag is starting with &#8220;h4ck1t{&#8220;. I made some manipulation on the script (Lot of replaces in Notepad++ to keep only the relevant char codes) to create a list of the char codes and then converted them to chars with python.

```python
>>> charcodes = [+20 +84,-52 +104,-2 +101,-7 +114,-52 +101,+43 +73,+8 +115,+1 +78,-63 +111,+22 +82,-10 +105,-55 +103,+8 +104,-49 +116,-17 +65,-42 +110,-49 +100,-34 +87,-19 +114,-40 +111,-62 +110,+13 +103,+49 +46,-35 +84,-26 +104,-48 +101,-65 +114,-33 +101,-28 +79,-15 +110,-31 +108,-16 +121,+8 +70,-66 +117,-15 +110,-12 +65,-61 +110,-33 +100,+9 +66,-16 +111,-37 +114,-56 +105,+0 +110,-35 +103,+79 +46]
>>> flag = ''
>>>
>>> for c in charcodes:
...     flag+=chr(c)
...
>>> print flag
h4ck1t{O0h_0pC0D35_G0t_1N51D3_MiN3_51CK_M1nD}
```


&nbsp;

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="./megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://bitbucket.org/xdasm/decompiler/issues/49/bcompiler-list