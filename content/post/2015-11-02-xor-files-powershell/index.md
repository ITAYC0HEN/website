---
title: XOR Files With Powershell
author: Megabeets
type: post
date: 2015-11-02T18:24:21+00:00
url: /xor-files-powershell/
categories:
  - Tools
tags:
  - Forensics
  - Powershell
  - XOR

---
Today I&#8217;m sharing with you one of the most simple and effective tool in my forensics-toolbox. A simple script, written in Powershell, that perform a logical exclusion, XOR, on two files and saves the result in the destination file. I used this tool several times for example  to  recover data from a broken RAID 5 or deobfuscate an obfuscated binary or image. The usage is very simple and intuitive.  
You can find the full code and examples in the [repository][1].

Have fun!

<pre class="toolbar:1 lang:ps decode:true" title="Powershell XOR Files">&lt;#
.DESCRIPTION
    Powershell XOR 2 Files

.EXAMPLE
    ./xor.ps1 C:\a.txt C:\b.txt C:\result.txt

.NOTES
    Author:  Itay Cohen
    Website: http://www.Megabeets.net
    Date:    Jul 2016    

.SYNOPSIS
    .
#&gt;


param (
    [Parameter(Mandatory=$true)]
    [string] $file1, #First File
    [Parameter(Mandatory=$true)]
    [string] $file2, #Second file
    [Parameter(Mandatory=$true)]
    [string] $out #Output File
) #end param

 
# Read two files as byte arrays
$file1_b = [System.IO.File]::ReadAllBytes("$file1") 
$file2_b = [System.IO.File]::ReadAllBytes("$file2")
 
# Set the length to be the smaller one
$len = if ($file1_b.Count -lt $file2_b.Count) {$file1_b.Count} else { $file2_b.Count}
$xord_byte_array = New-Object Byte[] $len

# XOR between the files
for($i=0; $i -lt $len ; $i++)
{
    $xord_byte_array[$i] = $file1_b[$i] -bxor $file2_b[$i]
}
 
# Write the XORd bytes to the output file
[System.IO.File]::WriteAllBytes("$out", $xord_byte_array)

write-host "[*] $file1 XOR $file2`n[*] Saved to " -nonewline;
Write-host "$out" -foregroundcolor yellow -nonewline; Write-host ".";</pre>

&nbsp;

<span style="font-size: 10pt;"><a href="http://www.megabeets.net/xor-files-python/">Click here for the Python version.</a></span>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://github.com/ITAYC0HEN/XOR-Files