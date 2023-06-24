---
title: '[TWCTF-2016: PPC] Make a Palindrome! Writeup'
author: Megabeets
type: posts
date: 2016-09-05T00:00:46+00:00

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


> Your task is to make a palindrome string by rearranging and concatenating given words. 
>      
> Input Format: N <Word_1> <Word_2> ... <Word_N>  
> Answer Format: Rearranged words separated by space.  
> Each words contain only lower case alphabet characters.  
>      
> Example Input: 3 ab cba c  
> Example Answer: ab c cba  
>      
>      
> You have to connect to ppc1.chal.ctf.westerns.tokyo:31111(TCP) to answer the problem.  
>      
> $ nc ppc1.chal.ctf.westerns.tokyo 31111  
> * Time limit is 3 minutes
> * The maximum number of words is 10.       
> * There are 30 cases. You can get flag 1 on case 1. You can get flag 2 on case 30.
> 
> * <a class="attachment" href="https://twctf7qygt6ujk.azureedge.n./samples.7z-663f3427d93add2142cbce3bdf1181107ae83866d60a2b8830e911612d080eaf" target="_blank">samples.7z</a> Server connection examples.

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

