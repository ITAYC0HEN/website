---
title: '[TWCTF-2016: Reverse] Reverse Box Writeup'
author: Megabeets
type: post
date: 2016-09-04T23:58:28+00:00
url: /twctf-2016-reverse-reverse-box/
categories:
  - TWCTF 2016
  - Writeups
tags:
  - CTF
  - Reverse
  - TWCTF
  - Writeups

---
Guest post by Shak.

> **Challenge description**  
> $ ./reverse_box ${FLAG}  
> <span style="font-weight: 400;">95eeaf95ef94234999582f722f492f72b19a7aaf72e6e776b57aee722fe77ab5ad9aaeb156729676ae7a236d99b1df4a</span><span style="font-weight: 400;"><br /> </span>[<span style="font-weight: 400;">reverse_box.7z</span>][1]

* * *

<span style="font-weight: 400;">This challenge is a binary which expects one argument and then spits out a string. We get the output of the binary for running it with the flag. Let’s fiddle with that for start.</span>

<pre class="theme:plain-white lang:sh decode:true ">./reverse_box 0000
28282828</pre>

<span style="font-weight: 400;">The binary probably prints a hex value replacing each character. It is also clear that it is a unique value for each character. we must be dealing with some kind of substitution cipher. After running it again with the exact same argument, we get a different output, so the substitution is also randomized somehow. </span>

<span style="font-weight: 400;">Jumping into the binary, we are presented with 2 important functions which i named main and calculate.</span>

<pre class="toolbar:1 lang:c decode:true" title="From Hex-Rays Decompiler">int __cdecl main(int argc, const char **argv, const char **envp)
{
  int result; // eax@7
  int v4; // ecx@7
  size_t i; // [sp+18h] [bp-10Ch]@4
  int v6; // [sp+1Ch] [bp-108h]@4
  int v7; // [sp+11Ch] [bp-8h]@1

  v7 = *MK_FP(__GS__, 20);
  if ( argc &lt;= 1 )
  {
    printf("usage: %s flag\n", *argv);
    exit(1);
  }
  calculate((int)&v6);
  for ( i = 0; i &lt; strlen(argv[1]); ++i )
    printf("%02x", *((_BYTE *)&v6 + argv[1][i]));
  putchar('\n');
  result = 0;
  v4 = *MK_FP(__GS__, 20) ^ v7;
  return result;
}
</pre>

<span style="font-weight: 400;">The main function is pretty straightforward. It is responsible for checking whether we run the binary with an argument, calling the calculate function, and finally printing the result. Looking the result printing format we can see that we were right with the result being in a hexadecimal format. </span>

<span style="font-weight: 400;">Next we’ll deal with the calculate function.</span>

<pre class="toolbar:1 lang:c decode:true " title="From Hex-Rays Decompiler">int __cdecl calculate(int a1)
{
  unsigned int seed; // eax@1
  int v2; // edx@4
  char v3; // al@5
  char v4; // ST1B_1@7
  char v5; // al@8
  int v6; // eax@10
  int v7; // ecx@10
  int v8; // eax@10
  int v9; // ecx@10
  int v10; // eax@10
  int v11; // ecx@10
  int v12; // eax@10
  int v13; // ecx@10
  int result; // eax@10
  char v15; // [sp+1Ah] [bp-Eh]@3
  char v16; // [sp+1Bh] [bp-Dh]@3
  char v17; // [sp+1Bh] [bp-Dh]@7
  int randomNum; // [sp+1Ch] [bp-Ch]@2

  seed = time(0);
  srand(seed);
  do
    randomNum = (unsigned __int8)rand();
  while ( !randomNum );
  *(_BYTE *)a1 = randomNum;
  v15 = 1;
  v16 = 1;
  do
  {
    v2 = (unsigned __int8)v15 ^ 2 * (unsigned __int8)v15;
    if ( v15 &gt;= 0 )
      v3 = 0;
    else
      v3 = 27;
    v15 = v2 ^ v3;
    v4 = 4 * (2 * v16 ^ v16) ^ 2 * v16 ^ v16;
    v17 = 16 * v4 ^ v4;
    if ( v17 &gt;= 0 )
      v5 = 0;
    else
      v5 = 9;
    v16 = v17 ^ v5;
    v6 = *(_BYTE *)a1;
    LOBYTE(v6) = v16 ^ v6;
    v7 = (unsigned __int8)v16;
    LOBYTE(v7) = __ROR1__(v16, 7);
    v8 = v7 ^ v6;
    v9 = (unsigned __int8)v16;
    LOBYTE(v9) = __ROR1__(v16, 6);
    v10 = v9 ^ v8;
    v11 = (unsigned __int8)v16;
    LOBYTE(v11) = __ROR1__(v16, 5);
    v12 = v11 ^ v10;
    v13 = (unsigned __int8)v16;
    LOBYTE(v13) = __ROR1__(v16, 4);
    result = v13 ^ v12;
    *(_BYTE *)(a1 + (unsigned __int8)v15) = result;
  }
  while ( v15 != 1 );
  return result;
}
</pre>

<span style="font-weight: 400;">It is not at all as straightforward as our main, but basically it randomizes a variable, does some memory operations and returns some random value. For now, let’s try debugging the binary without fully understanding calculate. </span>

<span style="font-weight: 400;">The binary iterates the characters in our input and executes the following assembly for printing the result.</span>

<pre class="lang:asm decode:true ">movzx   eax, byte ptr [esp+eax+1Ch]
movzx   eax, al
mov     [esp+4], eax
mov     dword ptr [esp], offset a02x ; "%02x"
call    _printf</pre>

<span style="font-weight: 400;">Arriving at this set of instructions, the eax register is holding a character from our input. What is then passed for printf function is an element from an array in [esp+1Ch] at the eax location (line 1). So this array holds our result. Let&#8217;s further examine it and see what it’s all about. Jumping that location on the stack we encounter a 268 Bytes array of hex values, we will save the array to a binary file called </span>_<span style="font-weight: 400;">array</span>_ <span style="font-weight: 400;">for later. </span>

<span style="font-weight: 400;">After running the binary a couple of times, we can understand that the array is also randomized somehow. Maybe it has something to do with the random value that the calculate function is returns. Running the binary a couple of times more, we see that the second element of the array is always equal to the random number from calculate. So, the binary must have a base array which is then manipulates somehow with the random value. The second element in the base array must be a zero if every time the manipulation occurs we get the random number. Let’s try and understand what kind of manipulation the binary performs. We will do that by loading the binary array to a python script, then we will get the base array by performing the assumed manipulation on our saved array elements with the known random value from our execution of the binary, and finally comparing the process to another binary run with a different random value for verification. The first option will be adding the random value to the base array value. It works for the second element, but comparing this process to another binary run does not work. The second guess will be that the binary is XORing the random value with the base array element which will also allow the second array element to be equal to the random value. Bingo, this works. Now all we have to do is get the base array using our saved array and the random value for that binary run. Next thing we will have to consider is the flag structure which is TWCTF{&#8230;.}. We can calculate what was the random value when they run the flag through the binary and finally check where was every hex value from the flag result and get the original output by getting the char representation for the location.</span>

<pre class="lang:python decode:true ">#! /usr/bin/python
f = open(r"c:\megabeets\array", "rb")
buff = f.read(268)
random_value = 0x66
base_array = []

#Claculate the base array
for i in buff:
	base_array.append(ord(i) ^ random value)

#The given output from the flag run, each value is seperated by -
flag_output = "95-ee-af-95-ef-94-23-49-99-58-2f-72-2f-49-2f-72-b1-9a-7a-af-72-e6-e7-76-b5-7a-ee-72-2f-e7-7a-b5-ad-9a-ae-b1-56-72-96-76-ae-7a-23-6d-99-b1-df-4a"
flag = ''
for c in flag_output:
	flag.append(int('0x' + c , 16))

T_location = ord('T')
#XORing the T_location element in the base array with the output result in order to get the random value
flag_random_value = base_array[T_location] ^ flag[0]

#Manipulate the base array to create the array for the flag binary run
flag_array = []
for b in base_array:
	flag_array.append(ord(b) ^ flag_random_value)

# Create a dictionary which maps an output hex value to an input character 
dic{}
for i in range(0, 268):
	dic[flag_array[i]] = chr(i)

#
for i in xrange(len(flag)):
	print dic[flag[i]],
</pre>

<span style="font-weight: 400;">et voilà</span><span style="font-weight: 400;">, the flag is <span style="font-size: 10pt;">TWCTF{5UBS717U710N_C1PH3R_W17H_R4ND0M123D_5-B0X}.</span></span>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://twctf7qygt6ujk.azureedge.net/uploads/reverse_box.7z-f1ffb64d2a0848fdccd02ed63f0f2de6937545fa294ee530d73bf2c1fec27691