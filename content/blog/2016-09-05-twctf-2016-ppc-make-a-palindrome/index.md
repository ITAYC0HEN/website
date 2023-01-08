---
title: '[TWCTF-2016: PPC] Make a Palindrome! Writeup'
author: Megabeets
type: post
date: 2016-09-05T00:00:46+00:00
url: /twctf-2016-ppc-make-a-palindrome/
categories:
  - TWCTF 2016
  - Writeups
tags:
  - CTF
  - PPC
  - TWCTF
  - Writeups

---
**Challenge description:**

<table>
  <tr>
    <td>
      <em>Your task is to make a palindrome string by rearranging and concatenating given words.</em></p> 
      
      <pre>Input Format: N <Word_1> <Word_2> ... <Word_N>
Answer Format: Rearranged words separated by space.
Each words contain only lower case alphabet characters.

Example Input: 3 ab cba c
Example Answer: ab c cba

```

      
      <p>
        <em>You have to connect to ppc1.chal.ctf.westerns.tokyo:31111(TCP) to answer the problem.</em>
      </p>
      
      <pre>$ nc ppc1.chal.ctf.westerns.tokyo 31111

```

      
      <ul>
        <li>
          <em>Time limit is 3 minutes.</em>
        </li>
        <li>
          <em>The maximum number of words is 10.</em>
        </li>
        <li>
          <em>There are 30 cases. You can get flag 1 on case 1. You can get flag 2 on case 30.</em>
        </li>
        <li>
          <em><a class="attachment" href="https://twctf7qygt6ujk.azureedge.n./samples.7z-663f3427d93add2142cbce3bdf1181107ae83866d60a2b8830e911612d080eaf" target="_blank">samples.7z</a> Server connection examples.</em>
        </li>
      </ul>
    </td>
  </tr>
</table>

* * *

This challenge was pretty simple. I used the given &#8220;_example.py&#8221;_  and added the following function to it:

```python
def makepal(l):
	for b in itertools.permutations(l, len(l)):
		str1 = ''.join(b)
		if str(str1) == str(str1)[::-1]:
			print b
			return b
```


Then I called it using the original script structure:

```python
answer = makepal(words)
```


And the server returned the flag:  
**TWCTF{Hiyokko_Tsuppari}**

The full script can be found <a href="https://gist.github.com/ITAYC0HEN/b88bfd6578bde4dcbe236f8ea67f24b7" target="_blank">here</a>.

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="./megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>