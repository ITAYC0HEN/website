---
title: XOR Files With Python
author: Megabeets
type: post
date: 2015-11-27T08:08:59+00:00
url: /xor-files-python/
categories:
  - Tools
tags:
  - Forensics
  - Python
  - XOR

---
This is a simple script, written in Python, that perform a logical exclusion, XOR, on two files and saves the result in the destination file. It is one of the most simple and effective tool in my forensics-toolbox. I used this tool several times for example to recover data from a broken RAID 5 or deobfuscate an obfuscated binary or image. The usage is very simple and intuitive.  
You can find the full code and examples in the [repository][1].

Have fun!

<pre class="toolbar:1 lang:python decode:true " title="Python XOR Files">#######################
# Powershell XOR 2 Files
# xor.py
# Jul 2016
# Website: http://www.Megabeets.net
# Use: ./xor.py file1 file2 outputFile
# Example: ./xor.py C:\a.txt C:\b.txt C:\result.txt
#######################

import sys

# Read two files as byte arrays
file1_b = bytearray(open(sys.argv[1], 'rb').read())
file2_b = bytearray(open(sys.argv[2], 'rb').read())

# Set the length to be the smaller one
size = len(file1_b) if len(file1_b) &lt; len(file2_b) else len(file2_b)
xord_byte_array = bytearray(size)

# XOR between the files
for i in range(size):
	xord_byte_array[i] = file1_b[i] ^ file2_b[i]

# Write the XORd bytes to the output file	
open(sys.argv[3], 'wb').write(xord_byte_array)

print "[*] %s XOR %s\n[*] Saved to \033[1;33m%s\033[1;m."%(sys.argv[1], sys.argv[2], sys.argv[3])</pre>

&nbsp;

[<span style="font-size: 10pt;">Click here for the Powershell Version.</span>][2]

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://github.com/ITAYC0HEN/XOR-Files
 [2]: https://www.megabeets.net/xor-files-powershell/