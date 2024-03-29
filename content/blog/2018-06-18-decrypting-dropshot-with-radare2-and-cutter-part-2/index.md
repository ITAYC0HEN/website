---
title: Decrypting APT33’s Dropshot Malware with Radare2 and Cutter – Part 2
author: Megabeets
type: posts
date: 2018-06-18T15:37:22+00:00

categories:
  - Articles
  - Malware
  - radare2
tags:
  - Malware
  - radare2
  - Reverse

---
##  Prologue

Previously, in the first part of this article, we used Cutter, a GUI for radare2, to statically analyze APT33&#8217;s Dropshot malware. We also used radare2&#8217;s Python scripting capabilities in order to decrypt encrypted strings in Dropshot. If you didn&#8217;t read the first part yet, I suggest you do it [now][1].

Today&#8217;s article will be shorter, now that we are familiar with cutter and r2pipe, we can quickly analyze another interesting component of Dropshot &#8212; an encrypted resource that includes Dropshot&#8217;s actual payload. So without further ado, let&#8217;s start.

[<img src="./cutter_logo_trimmed.png" />][2]



## Downloading and installing Cutter

Cutter is available for all platforms (Linux, OS X, Windows). You can download the latest release [here][3]. If you are using Linux, the fastest way to use Cutter is to use the AppImage file.

If you want to use the newest version available, with new features and bug fixes, you should build Cutter from source by yourself. It isn&#8217;t a complicated task and it is the version I use.

First, you must clone the repository:

```default
git clone --recurse-submodules https://github.com/radareorg/cutter
cd cutter
```


Building on Linux:

  ```default
./build.sh
```

  
Building on Windows:
  
```default
prepare_r2.bat
build.bat
```


If any of those do not work, check the more detailed instruction page [here][4]



## Dropshot \ StoneDrill

As in the last part, we&#8217;ll analyze Dropshot, which is also known by the name StoneDrill. It is a wiper malware associated with the APT33 group which targeted mostly organizations in Saudi Arabia. Dropshot is a sophisticated malware sample, that employed advanced anti-emulation techniques and has a lot of interesting functionalities. The malware is most likely related to the infamous [Shamoon malware][5]. Dropshot was analyzed thoroughly by [Kaspersky][6] and later on by [FireEye][7]. In this article, we&#8217;ll focus on decrypting the encrypted resource of Dropshot which contains the actual payload of the malware.

The Dropshot sample can be downloaded from [here][8] <span style="font-size: 12pt;">(password: <em>infected</em>). I suggest you star (<span style="color: #ffcc00;">★</span>) <a href="https://github.com/ITAYC0HEN/A-journey-into-Radare2/">the repository</a> to get updates on more radare2 tutorials 🙂</span>

<span style="color: #ff0000;"><strong>Please, be careful when using this sample. It is a real malware, and more than that, a wiper! Use with caution!</strong></span>

_Since we&#8217;ll analyze Dropshot statically, you can use a Linux machine, as I did._



<!--more-->

## Getting Started

Assuming you went through the first part of the article, you are already familiar with Cutter and r2pipe. Moreover, you should already have a basic clue of how Dropshot behaves. Open the Dropshot sample in Cutter, execute in Jupyter the r2pipe script we wrote and seek to the \`main\` function using the &#8220;Functions&#8221; widget or the upper search bar.

<div id="attachment_1540" style="width: 697px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./cutter_dropshot_loadnt.png"><img src="./cutter_dropshot_loadnt.png" /></a>
  
  <p id="caption-attachment-1540" class="wp-caption-text">
    A function we analyzed in the previous article | Click to enlarge
  </p>
</div>

## main ()

The [role of the `main()` function][9] in a program shouldn&#8217;t be new to you since it is one of the fundamental concepts of programming. Using the Graph mode, we&#8217;ll go thorugh `main`&#8216;s flow in order to find our target &#8211; the resource decryption routine. We can see that a function at `0x403b30` is being called at the first block of `main`.

[<img src="./dropshot_cutter_main.png" />][10]

Double-clicking this line will take us to the graph of `fcn.00403b30`, a rather big function. Going through this function, we&#8217;ll see some non-sense Windows API calls with invalid arguments. When describing Dropshot, I said that it uses anti-emulation heavily &#8211; this function, for example, performs anti-emulation.

<div id="attachment_1523" style="width: 626px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./dropshot_anti_emulation_1.png"><img aria-describedby="caption-attachment-1523" data-attachment-id="1523" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/dropshot_anti_emulation_1/#main" data-orig-file="http://www.megabeets.n./dropshot_anti_emulation_1.png" data-orig-size="626,584" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="dropshot_anti_emulation_1" data-image-description="" data-image-caption="<p>Click to enlarge</p>
" data-medium-file="http://www.megabeets.n./dropshot_anti_emulation_1.png" data-large-file="http://www.megabeets.n./dropshot_anti_emulation_1.png" decoding="async" loading="lazy" class="size-full wp-image-1523" src="https://www.megabeets.n./dropshot_anti_emulation_1.png" alt="" width="626" height="584" srcset="https://www.megabeets.n./dropshot_anti_emulation_1.png 626w, https://www.megabeets.n./dropshot_anti_emulation_1.png 150w, https://www.megabeets.n./dropshot_anti_emulation_1.png 300w" sizes="(max-width: 626px) 100vw, 626px" /></a>
  
  <p id="caption-attachment-1523" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

### Anti-Emulation

Anti-emulation techniques are used to fool the emulators of anti-malware products. The emulators are one of the most important components of many security products. Among others, they are used to analyze the behavior of malware, unpack samples and to analyze shellcode. It is doing this by emulating the program&#8217;s workflow by mimicking the target architecture&#8217;s instruction set, as well as the running environment, and dozens or even hundreds of popular API functions. All this is done in order to make a malware ‘think’ it has been executed in a real environment by a victim user.

The emulator engine is mimicking the API or the system calls that are offered by the actual operating systems. Usually, it will implement popular API functions from libraries such as _user32.dll_, _kernel32.dll_, and _ntdll.dll_. Most of the times this will be a dummy implementation where the fake functions won&#8217;t really do anything except returning a successful return value.

<p style="direction: ltr;">
  By using different anti-emulation techniques, malware authors are trying to fool a generic or even a specific emulator. The most common technique, which is also implemented in Dropshot&#8217;s <code>fcn.00403b30</code>, is the use of uncommon or undocumented API calls. This technique can be improved by using incorrect arguments (like <em>NULL</em>) to a certain API function which should cause an Access Violation exception in a real environment.
</p>

In our case, Dropshot is calling some esoteric functions as well as passing non-sense arguments to different API functions.

<span style="font-size: 10pt;"><em>More information about emulation and anti-emulation mechanisms are available in the following, highly recommended, book: <a href="https://www.amazon.com/Antivirus-Hackers-Handbook-Joxean-Koret/dp/1119028752"><span id="productTitle" class="a-size-extra-large">The Antivirus Hacker&#8217;s Handbook</span></a></em></span>

Now that we know all this, we can rename this function from `fcn.00403b30` to a more meaningful name. I used &#8220;AntiEmulation&#8221; but you can choose whatever name you want, as long as it is meaningful to you. Clicking the call instruction and then pressing Shift+N will open the Rename dialog box. Right-clicking the row and choosing &#8220;Rename&#8221; will do the job as well.

[<img src="./rename_function_AntiEmulation.png" />][11]After `main` is calling to the AntiEmulation function, we are facing a branch. Here&#8217;s the assembly, copied from Cutter&#8217;s Disassembly widget:

```sh
|           0x004041a6           call AntiEmulation
|           0x004041ab           mov eax, 1
|           0x004041b0           test eax, eax
|       ,=< 0x004041b2           je 0x40429d
|       |   0x004041b8           push 4
```


As you can see, the code would never branch to `0x40429d` since this `test eax, eax` followed by `je ...` is basically checking whether `eax` equals 0. One instruction before, the program moved the value 1 to `eax`, thus `0x40429d` would be [The Road Not Taken][12].

We&#8217;ll skip the next block which is responsible for creating temporary files and take a look at the block starting at `0x4041f9`. In this block, we&#8217;ll see that Dropshot is creating a modeless Dialog box using [`CreateDialogParamA`][13] with the following parameters: `CreateDialogParamA(0, 0x410, 0, DialogFunc, 0);`.

[<img src="./main_CreateDialog.png" />][14]

The DialogPrc callback which is passed to `CreateDialogParamA` is recognized by radare2 and shown by Cutter as `fcn.DialogFunc`. This function contains the main logic of the dropper and this is the function that we&#8217;ll focus on. Later in this block, `ShowWindow` is being called in order to &#8220;show&#8221; the window. Obviously, this is a dummy window which would never be shown since the malware author doesn&#8217;t want any artifact to be shown to the victim. `ShowWindow` will trigger the execution of `fcn.DialogFunc`.

Double-clikcing on `fcn.DialogFunc` will take us to the function itself. We can see that it is performing several comparisons for the messages it receives and then is calling to a very interesting function `fcn.00403240`.



## Handling  the Resources

The first block of `fcn.00403240` is pretty straightforward. Dropshot is getting a handle to itself using `GetModuleHandleA`. Then, by using `FindResourceA`, it is locating a resource with a dummy type 0x67 (Decimal: 111) and a name 0x6e (Decimal: 110). Finally, it is loading a resource with this name using `LoadResource`.

[<img src="./first_bb_load_dummy_rsrc.png" />][15]

Using Cutter, we can see the content of this resource. Simply go to the Resources widget and locate the resource with the name &#8220;110&#8221;.

[<img src="./Cutter_Resources_window1.png" />][16]As you can see in the screenshot above, the size of the resource is 28 Bytes and its Lang is Farsi which might hint us about the threat actor behind Dropshot.

Double-clicking the resource will take us to the resource&#8217;s address. Let&#8217;s click on the Hexdump widget to see its data. In the hexdump we can see that this resource contains the following bytes: `01 00 00 00 00 00 00 00 01 00 00 00 00 00 00 00 01 00 00 00 01 00 00 00 00 00 00 00`. Those of you who are familiar with radare2 may use the Console widget in the bottom left to do this quickly with `px`:

[<img src="./cutter_console_px28.png" />][17]This resource will be used later but we won&#8217;t be getting into it since it is out of the scope of this post.

After loading the resource, in the next block we can see the start of a loop:

[<img src="./cutter_dummy_loop_lod.png" />][18]This loop is checking if `local_2ch` equals to 0x270f (Decimal: 9999) and if yes it exits from the loop. Inside this loop, there will be another loop of 999 iterations. So basically, this is how this nested loop looks like:

```c
for ( i = 0; i < 9999; ++i )
  {
    for ( j = 0; j < 999; ++j )
    {
        dummy_code;
    }
  }
```


<p style="direction: ltr;">
  This is just another anti-emulation\analysis technique which is basically doing nothing. This is another example of the heavy use of anti-emulation by Dropshot.
</p>

After this loop, the right branch is taken and this is an interesting one.

<div id="attachment_1499" style="width: 500px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./tdecrypt_2ndbranch.png"><img src="./tdecrypt_2ndbranch.png" /></a>
  
  <p id="caption-attachment-1499" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

At first, `VirutalAlloc` in the size of 512 bytes is being called. Next, `fcn.00401560` is called with 3 arguments. Let&#8217;s enter this function and see what&#8217;s in there:

<div id="attachment_1500" style="width: 912px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./fcn00401560.png"><img src="./fcn00401560.png" /></a>
  
  <p id="caption-attachment-1500" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

Hey! Look who&#8217;s back! We can see the 2 functions we analyzed in the previous article: `decryption_function` and `load_ntdll_or_kernel32`. That&#8217;s great! Also, you can notice the comment on `call decryption_function` which is telling us that the decrypted string is `GetModuleFileNameW`. This comment is the result of the r2pipe script we wrote.

In this function, `GetModuleFileNameW` will be decrypted, then Kernel32.dll will be loaded and `GetProcAddress` will be called to get the address of `GetModuleFileNameW` and then it will move it to `[0x41dc04]`. Later in this function, `[0x41dc04]` will be called.

Basically, this function is wrapper around `GetModuleFileNameW`, something which is common in Dropshot&#8217;s code. Let&#8217;s rename this function to `w_GetModuleFileNameW` where &#8220;w_&#8221; stands for &#8220;wrapper&#8221;. Of course, you can choose whatever naming convention you prefer.

Right after the call to `w_GetModuleFileNameW`, Dropshot is using VirtualAlloc to allocate 20 (0x14) bytes, then there is a call to `fcn.00401a40` which is a function quite similar to [`memset`][19], it is given with 3 arguments (_address, value_ and _size_), just like `memset` and it is responsible to fill the range from `address` to `address+size` with the given `value`. Usually, along the program, this function is used to fill an allocated buffer with zeroes. This is quite strange to me, since VirtualAlloc is already &#8220;initializes the memory it allocates to zero&#8221;. Let&#8217;s name this function to `memset_` using Shift+N or via right click and move on.

Right after the program is zeroing-out the allocated memory, we see a call to another function &#8211; `fcn.00401c90`. We can see 3 arguments which are being passed to it, 0x14, 0x66, and 0x68. Since sometimes we prefer to see decimal numbers and not hex, let&#8217;s use another useful trick of Cutter. Right-click on any of these hex numbers and choose &#8220;Set Immediate Base to&#8230;&#8221; and then select &#8220;Decimal&#8221;.

[<img src="./cutter_set_imm_base.png" />][20]Now we can see that the values which are being passed to `fcn.00401c90` are 20, 102 and 104. Looks familiar? 20 was the size of the buffer that was just allocated. 102 and 104 remind us the Resource name and type that used before (110 and 111). Are we dealing with resources here? Let&#8217;s see.

Moving to the Resources widget again, we can see that there&#8217;s indeed a resource named &#8220;102&#8221; which &#8220;104&#8221; is its type. And yes, it is 19B long, close enough 😉

`fcn.00401c90`is one of the key functions involved in the dropper functionality of Dropshot. The thing is, that this function is rather big and quite complicated when you don&#8217;t know how to look at it. We&#8217;ll get back to it in one minute but before that, I want to show you an approach I use while reverse engineering some pieces of code and while facing a chain of calls to functions which are probably related to each other.

First, we saw that 20 bytes were allocated by `VirtualAlloc` and the pointer to the allocated memory was moved to `[local_ch]`. Right after that, `memset_` was called in order to zero-out 20 bytes at`[local_ch]`, i.e to zero-out the allocated buffer. Immediately after, `fcn.00401c90` was called and 3 arguments were passed to it &#8211; 104, 102 and our beloved 20. We know that 102 is a name of a resource and its size is almost 20. We don&#8217;t know yet what this function is doing but we know that its return value(`eax`) is being passed along with 2 more arguments to another function, `fcn.00401a80`. The other arguments are, you guessed right, 20 and the allocated buffer. A quick look at `fcn.00401a80`, which is a really tiny function, will reveal us that this function is copying a buffer to the allocated memory. This function is quite similar to [`memcpy`][21] so we&#8217;ll rename it to `memcpy_`. So now we can do an educated guess and say that `fcn.00401c90` is reading a resource to a buffer and returns a pointer to it.

Using this approach, we can understand (or at least guess) a complicated function without even analyzing it. Just by looking at a programs chain of function calls, we can build the puzzle and save us important time.

That said, we&#8217;ll still give this function a quick analysis because we want to be sure that we guessed right, and more importantly &#8212; because this is an interesting function.

## Resource Parser

The next part is where things are getting more complicated. We&#8217;ll start by going over `fcn.00401c90` pretty fast so try to follow. Also, you may want to make sure you fasten yourself since we are going on a rollercoaster ride through the PE structure.

Take a look at the first block of this function. You&#8217;ll see one `call` and a lot of `mov`, `add` and calculation of offsets. This is how a typical PE parsing looks like.

```asm
/ (fcn) fcn.00401c90 468
|   fcn.00401c90 (int arg_8h, int arg_ch, int arg_10h);
|           0x00401c90           push ebp
|           0x00401c91           mov ebp, esp
|           0x00401c93           sub esp, 0x44
|           0x00401c96           mov dword [local_40h], 0
|           0x00401c9d           push 0
|           0x00401c9f           call GetModuleHandleW
|           0x00401ca5           mov dword [local_34h], eax
|           0x00401ca8           mov eax, dword [local_34h]
|           0x00401cab           mov dword [local_20h], eax
|           0x00401cae           mov ecx, dword [local_20h]
|           0x00401cb1           mov dword [local_24h], ecx
|           0x00401cb4           mov edx, dword [local_24h]
|           0x00401cb7           mov eax, dword [edx + 0x3c]
|           0x00401cba           add eax, dword [local_24h]
|           0x00401cbd           mov dword [local_38h], eax
|           0x00401cc0           mov ecx, dword [local_38h]
|           0x00401cc3           add ecx, 0x18
|           0x00401cc6           mov dword [local_3ch], ecx
|           0x00401cc9           mov edx, dword [local_3ch]
|           0x00401ccc           add edx, 0x60
|           0x00401ccf           mov dword [local_28h], edx
|           0x00401cd2           mov eax, 8
|           0x00401cd7           shl eax, 1
|           0x00401cd9           mov ecx, dword [local_28h]
|           0x00401cdc           mov edx, dword [ecx + eax]
|           0x00401cdf           mov dword [local_44h], edx
|           0x00401ce2           mov eax, 8
|           0x00401ce7           shl eax, 1
|           0x00401ce9           mov ecx, dword [local_28h]
|           0x00401cec           mov edx, dword [local_20h]
|           0x00401cef           add edx, dword [ecx + eax]
|           0x00401cf2           mov dword [local_10h], edx
|           0x00401cf5           mov eax, dword [local_10h]
|           0x00401cf8           mov dword [local_4h], eax
|           0x00401cfb           mov ecx, dword [local_4h]
|           0x00401cfe           movzx edx, word [ecx + 0xe]
|           0x00401d02           mov eax, dword [local_4h]
|           0x00401d05           movzx ecx, word [eax + 0xc]
|           0x00401d09           add edx, ecx
|           0x00401d0b           mov dword [local_ch], edx
|           0x00401d0e           mov dword [local_14h], 0
|       ,=< 0x00401d15           jmp 0x401d20
...
...
```


At first, a handle to the current process is received using `GetModuleHandleW`. Then, the handle (`eax`) is being moved to a variety of local variables. First, it is being moved to `[local_34h]` at `0x00401ca5`. Then you can see `eax` moved to `[local_20h]` which is later being moved to `[local_24h]` using `ecx`.

So basically we have a bunch of local variables that currently hold the handle `hmodule`. We can rename all three variables to `[hmodule_x]` so it&#8217;ll be easier to keep track of all the reference to `hmodule`. To rename flags you can use the Console widget and just execute `afvn old_name new_name`. For example, I executed: `afvn local_34h hmodule_1; afvn local_20h hmodule_2; afvn local_24h hmodule_3`.

`GetModuleHandle` returns a handle to a mapped module, this basically means that our `hmodule`s point to our binary&#8217;s base address. In line `0x00401cb4` we can see that `[hmodule_3]` is moved to `edx`, then the value at `[edx + 0x3c]` is being moved to `eax` and `[hmodule_3]` is added to it at `0x00401cba`. Finally, `eax` is moved to `[local_38h]`. To put it simply, we can use the following pseudo-code:

```c
[local_38h] = (BYTE*)hmodule + *(hmodule + 0x3c)
```


So what&#8217;s in this address? Use your favorite binary structure viewer to find out. In this example, I&#8217;ll use [PEview][22] but you can use any other program you prefer &#8211; including the binary structure parsing feature of radare2, if you&#8217;re already a radare2 pro (see [`pf?`][23]).

Open Dropshot in PEview and inspect the DOS Header:

[<img src="./cutter-peview_dos_header.png" />][24]As you can see, in offset `0x3c` there is a pointer (0x108) to the offset to the new EXE Header which is basically the [IMAGE\_NT\_HEADER][25]. Awesome! So `[local_38h]` holds the address of the NT Header. Let&#8217;s rename it to `NT_HEADER` and move on.

At address `0x00401cc0` we can see that `NT_HEADER` is moved to `ecx` and then the program is adding 0x18 to `ecx`. Last, the value in `ecx` is moved to `[local_3ch]`. Just as before, let&#8217;s open again our PE parser and check what is in `NT_HEADER + 0x18`. Adding 0x18 to 0x108 will give us 0x120. Let&#8217;s see what is in this offset:

<div id="attachment_1506" style="width: 945px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./cutter-peview_nt_header.png" target="_blank" rel="noopener noreferrer"><img src="./cutter-peview_nt_header.png" /></a>
  
  <p id="caption-attachment-1506" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

Nice! 0x120 is the offset of the [IMAGE\_OPTIONAL\_HEADER][26] as can be seen in the image above. Let&#8217;s rename `local_3ch` to `OPTIONAL_HEADER`. To cut a _long story short_, Dropshot is then parsing the [IMAGE\_DATA\_DIRECTORY][27] structure (OPTIONAL\_HEADER + 0x60), the RESOURCE\_TABLE, and last it iterates through the different resources and compares the resource type and the resource name to the function&#8217;s arguments. Finally, it uses `memcpy_` to copy the content of the required resource to a variable and returns this variable.

Now we can rename the function to `get_resource` and the arguments to the corresponding meaning of them by executing `afvn arg_8h arg_rsrc_type; afvn arg_ch arg_rsrc_name; afvn arg_10h arg_dwsize`.

Now that we are sure about what this function does, we can see where else it is referenced. Right-click on the function and choosing &#8220;Show X-Refs&#8221; (or simply pressing &#8216;x&#8217;) will take us to the X-Refs window. We can see that `get_resource` is being called from two locations. One (`0x0040336d`) is already familiar to us, it is called to get resource &#8220;102&#8221;. The second call (at `0x00403439`), a few instructions later, is new to us &#8212; it is called to get the content of another resource, named &#8220;101&#8221; (0x65).

[<img src="./x-refs_to_get_resource.png" />][28]

Remember the screenshot of the Resources widget from before? We can see there the resource named &#8220;101&#8221;. What makes &#8220;101&#8221; so interesting is that it is **much** bigger than the other &#8212; its size is 69.6 KB! This is Dropshot&#8217;s payload. By going to the Resources widget and double-clicking &#8220;101&#8221; will take us to the resource&#8217;s offset in the binary. In the Hexdump widget, we can see that the content of this resource makes no sense and has a really high entropy (7.8 out of the maximum 8):

<div id="attachment_1509" style="width: 697px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./resource_101_content_and_entropy.png"><img src="./resource_101_content_and_entropy.png" /></a>
  
  <p id="caption-attachment-1509" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

This data is compressed/encrypted somehow so we need to decrypt it. Let&#8217;s continue our analysis to find out how.

## How To Decrypt The Resource

In order to decrypt the resource, we should follow the program&#8217;s flow to see how and where the payload is being used. Right after `get_resource` is being called with &#8220;101&#8221; and &#8220;103&#8221;, the resource is copied to `[local_20h]` using `memcpy_` (at `0x00403446`). Let&#8217;s call it `compressed_payload`. The compressed buffer is then passed to `fcn.00401e70` which is a function that performs dummy math calculations on the resource&#8217;s data. Probably another Anti-Emulation technique or simply a way to waste our time. I&#8217;ll rename it to `dummy_math`. Next, `compressed_payload` is being passed to `fcn.00401ef0` along with another buffer `[local_54h]`.

The analysis of this function is out of the scope of this article but this function is responsible to decompress a buffer using [zlib][29] and put the decompressed buffer in `[local_54h]`. You can see, for example, that `fcn.00401ef0` is calling to `fcn.004072f0` which contains strings like &#8220;unknown compression method&#8221; and &#8220;invalid window size&#8221; which can be found in the file [inflate.c][30] in the zlib repository. I renamed `fcn.00401ef0` to `zlib_decompress` and `local_54h` to `decompressed_payload`.

I&#8217;ll tell you now that simply a decompression of the buffer isn&#8217;t enough since there&#8217;s still another simple decryption to do. Straight after the decompressing, we can see more of the Anti-Emulation which we are already familiar with. Finally, our decompressed buffer is being passed to `fcn.00402620`. This function is responsible for the last decryption of the resource and then it performs a notorious technique known as [&#8220;Process Hollowing&#8221;][31] or &#8220;RunPE&#8221; in order to execute the decrypted payload.

So how `fcn.00402620` decrypts the decompressed payload? Simply, it uses `ror 3` to rotate-right each byte in the decompressed buffer. 3 stands for the number of bits to rotate.

[<img src="./ror3_dropshot.png" />][32]

The rest of this function is interesting as well but it has nothing to do with decrypting the resource so I&#8217;ll leave it to you.

To sum things up, and before we adding the logic for the resource decryption inside the script we wrote in the previous article &#8211; let&#8217;s sketch how the decryption function should look like. It should be something like this:

```python
rsrc_101 = get_resource("101")

decompressed_payload = decompress(rsrc_101)

decrypted_payload = []

for b in decompressed_payload:
   decrypted_payload.append(ror3(b))
```


## Scripting time! Decrypting the resource

Scripting radare2 is really easy thanks to [r2pipe][33]. It is the best programming interface for radare2.

> The r2pipe APIs are based on a single r2 primitive found behind `r_core_cmd_str()` which is a function that accepts a string parameter describing the r2 command to run and returns a string with the result.

r2pipe supports many programming languages including [Python][34], [NodeJS][35], [Rust][36], [C][37], and others.

Luckily, Cutter is coming with the python bindings of `r2pipe` integrated into its Jupyter component. We’ll write an r2pipe script that will do the following:

  * Save the compressed resource into a variable
  * Decompress the resource using zlib
  * Perform ror3 on each byte in the decompressed payload
  * Save the decrypted resource to a file

Just as in the previous part, let&#8217;s go to the Jupyter widget and open the script we wrote when decrypted the strings (part1).

The first thing to do is to read the content of the encrypted and compressed resource to a file:

```python
rsrcs = cutter.cmdj('iRj')
rsrc_101 = {}

# Locate resource 101 and dump it to an array
for rsrc in rsrcs:
    if rsrc['name'] == 101:
        rsrc_101 = cutter.cmdj("pxj %d @ %d" %
                                (rsrc['size'], rsrc['vaddr']))
```


> `iR` was used to get the list of resources and their offsets in the file. Next, we iterate through the different resources untill we find a resource named &#8220;101&#8221;. Last, using `px` we are reading the resource&#8217;s bytes into a varibale. We appended `j` to the commands in order to get their output as JSON.

Next, we want to decompress the buffer using zlib. Python is coming with &#8220;zlib&#8221; library by default which is great news for us. Add `import zlib` to the top of the script and use this code to decompress the buffer:

```python
# Decompress the zlibbed array
decompressed_data = zlib.decompress(bytes(rsrc_101))
```


Now that our buffer is decompressed in `decompressed_data`, all we left to do is to perform right rotation on the data and save it to a file.

Define [the following][38] `ror` lambda:

```python
def ror(val, r_bits, max_bits): return \
    ((val & (2**max_bits-1)) >> r_bits % max_bits) | \
    (val << (max_bits-(r_bits % max_bits)) & (2**max_bits-1))
```


And use it in your code like this:

```default
decrypted_payload = []

# Decrypt the payload
for b in decompressed_data:
    decrypted_payload.append((ror(b, 3, 8)))
```


Last, save it to a file:

```python
# Write the payload (a PE binary) to a file
open(r'./decrypted_rsrc.bin', 'wb').write(bytearray(decrypted_payload))
```


Now let&#8217;s combine the script from the previous article to the one we created now and test it in Jupyter. The combined script should first decode the encrypted scripts, and then it should decrypt the resource and save it to the disk.

<p style="text-align: center;">
  <span style="color: #79de5b;"><strong>The final script can be found <a href="https://github.com/ITAYC0HEN/A-journey-into-Radare2/blob/master/Part%203%20-%20Malware%20analysis/decrypt_dropshot.py"><span style="font-size: 12pt;">here</span></a></strong></span>.
</p>

Copy it and paste it into your Jupyter notebook. You can also execute your version of the code to see if you got it right by yourself.

[<img src="./jupyter_final_output.png" />][39]

Seems like our script was executed successfully and &#8220;Saved the PE to ./decrypted_rsrc.bin&#8221;. Great!

The last thing we want to do is to open `decrypted_rsrc.bin` in a new instance of Cutter in order to verify that this is indeed a PE file and that we didn&#8217;t corrupt the file in some way.

<div id="attachment_1520" style="width: 889px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.n./cutter_dashboard_dropped_payload.png"><img src="./cutter_dashboard_dropped_payload.png" /></a>
  
  <p id="caption-attachment-1520" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

Awesome! Cutter recognized the file as PE and seems like the code is correctly interpreted. This binary we just decrypted and saved is the Wiper module of Dropshot &#8211; a quite interesting piece of malware on its own. This module, just as its dropper, is using heavy anti-emulation and similar technique to decrypt its strings. You can give it a try and analyze it on your own using Cutter, radare2, and r2pipe. Good Luck!

## Epilogue

Here comes to an end the second and the last part of this series about decrypting Dropshot with Cutter and r2pipe. We got familiar with Cutter, radare2 GUI, and wrote a decryption script in r2pipe’s Python binding. We also analyzed some components of APT33’s Dropshot, an advanced malware.

As always, please post comments to this post or message me [privately][40] if something is wrong, not accurate, needs further explanation or you simply don’t get it. Don’t hesitate to share your thoughts with me.

**Subscribe on the left if you want to get the next articles straight in your inbox.**



 [1]: https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-1/
 [2]: https://www.megabeets.n./cutter_logo_trimmed.png
 [3]: https://github.com/radareorg/cutter/releases
 [4]: https://github.com/radareorg/cutter/blob/master/docs/Compiling.md
 [5]: https://en.wikipedia.org/wiki/Shamoon
 [6]: https://app.box.com/s/olc867zxc9nkjzm3wkjwi0b0e2awahtn
 [7]: https://www.fireeye.com/blog/threat-research/2017/09/apt33-insights-into-iranian-cyber-espionage.html
 [8]: https://github.com/ITAYC0HEN/A-journey-into-Radare2/blob/master/Part%203%20-%20Malware%20analysis/dropshot.exe.zip
 [9]: http://en.cppreference.com/w/cpp/language/main_function
 [10]: https://www.megabeets.n./dropshot_cutter_main.png
 [11]: https://www.megabeets.n./rename_function_AntiEmulation.png
 [12]: https://www.poetryfoundation.org/poems/44272/the-road-not-taken
 [13]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms645445(v=vs.85).aspx
 [14]: https://www.megabeets.n./main_CreateDialog.png
 [15]: https://www.megabeets.n./first_bb_load_dummy_rsrc.png
 [16]: https://www.megabeets.n./Cutter_Resources_window1.png
 [17]: https://www.megabeets.n./cutter_console_px28.png
 [18]: https://www.megabeets.n./cutter_dummy_loop_lod.png
 [19]: http://www.cplusplus.com/reference/cstring/memset/
 [20]: https://www.megabeets.n./cutter_set_imm_base.png
 [21]: http://en.cppreference.com/w/c/string/byte/memcpy
 [22]: http://wjradburn.com/software/
 [23]: http://radare.today/posts/parsing-a-fileformat-with-radare2/
 [24]: https://www.megabeets.n./cutter-peview_dos_header.png
 [25]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms680336(v=vs.85).aspx
 [26]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms680339(v=vs.85).aspx
 [27]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms680305(v=vs.85).aspx
 [28]: https://www.megabeets.n./x-refs_to_get_resource.png
 [29]: https://zlib.net/
 [30]: https://github.com/madler/zlib/blob/master/inflate.c#L622
 [31]: https://attack.mitre.org/wiki/Technique/T1093
 [32]: https://www.megabeets.n./ror3_dropshot.png
 [33]: https://github.com/radare/radare2-r2pipe
 [34]: https://github.com/radare/radare2-r2pipe/tree/master/python
 [35]: https://github.com/radare/radare2-r2pipe/tree/master/nodejs/r2pipe
 [36]: https://github.com/radare/radare2-r2pipe/tree/master/rust
 [37]: https://github.com/radare/radare2-r2pipe/tree/master/c
 [38]: https://www.falatic.com/index.php/108/python-and-bitwise-rotation
 [39]: https://www.megabeets.n./jupyter_final_output.png
 [40]: https://www.megabeets.net/about.html#contact