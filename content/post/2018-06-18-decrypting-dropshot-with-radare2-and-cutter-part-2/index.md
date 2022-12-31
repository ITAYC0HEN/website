---
title: Decrypting APT33’s Dropshot Malware with Radare2 and Cutter – Part 2
author: Megabeets
type: post
date: 2018-06-18T15:37:22+00:00
url: /decrypting-dropshot-with-radare2-and-cutter-part-2/
categories:
  - Articles
  - Malware
  - radare2
tags:
  - Malware
  - radare2
  - Reverse

---
## <span class="ez-toc-section" id="_Prologue"></span> Prologue<span class="ez-toc-section-end"></span>

Previously, in the first part of this article, we used Cutter, a GUI for radare2, to statically analyze APT33&#8217;s Dropshot malware. We also used radare2&#8217;s Python scripting capabilities in order to decrypt encrypted strings in Dropshot. If you didn&#8217;t read the first part yet, I suggest you do it [now][1].

Today&#8217;s article will be shorter, now that we are familiar with cutter and r2pipe, we can quickly analyze another interesting component of Dropshot &#8212; an encrypted resource that includes Dropshot&#8217;s actual payload. So without further ado, let&#8217;s start.

[<img data-attachment-id="1530" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_logo_trimmed/#main" data-orig-file="http://www.megabeets.net/uploads/cutter_logo_trimmed.png" data-orig-size="600,260" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter_logo_trimmed" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter_logo_trimmed-300x130.png" data-large-file="http://www.megabeets.net/uploads/cutter_logo_trimmed.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1530" src="https://www.megabeets.net/uploads/cutter_logo_trimmed.png" alt="" width="600" height="260" srcset="https://www.megabeets.net/uploads/cutter_logo_trimmed.png 600w, https://www.megabeets.net/uploads/cutter_logo_trimmed-150x65.png 150w, https://www.megabeets.net/uploads/cutter_logo_trimmed-300x130.png 300w" sizes="(max-width: 600px) 100vw, 600px" />][2]

## 

## <span class="ez-toc-section" id="Downloading_and_installing_Cutter"></span>Downloading and installing Cutter<span class="ez-toc-section-end"></span>

Cutter is available for all platforms (Linux, OS X, Windows). You can download the latest release [here][3]. If you are using Linux, the fastest way to use Cutter is to use the AppImage file.

If you want to use the newest version available, with new features and bug fixes, you should build Cutter from source by yourself. It isn&#8217;t a complicated task and it is the version I use.

First, you must clone the repository:

<pre class="lang:default decode:true">git clone --recurse-submodules https://github.com/radareorg/cutter
cd cutter</pre>

Building on Linux:

<div class="highlight highlight-source-shell">
  <pre class="lang:default decode:true">./build.sh</pre>
  
  <p>
    Building on Windows:
  </p>
  
  <pre class="lang:default decode:true">prepare_r2.bat
build.bat</pre>
</div>

If any of those do not work, check the more detailed instruction page [here][4]

## 

## <span class="ez-toc-section" id="Dropshot_StoneDrill"></span>Dropshot \ StoneDrill<span class="ez-toc-section-end"></span>

As in the last part, we&#8217;ll analyze Dropshot, which is also known by the name StoneDrill. It is a wiper malware associated with the APT33 group which targeted mostly organizations in Saudi Arabia. Dropshot is a sophisticated malware sample, that employed advanced anti-emulation techniques and has a lot of interesting functionalities. The malware is most likely related to the infamous [Shamoon malware][5]. Dropshot was analyzed thoroughly by [Kaspersky][6] and later on by [FireEye][7]. In this article, we&#8217;ll focus on decrypting the encrypted resource of Dropshot which contains the actual payload of the malware.

The Dropshot sample can be downloaded from [here][8] <span style="font-size: 12pt;">(password: <em>infected</em>). I suggest you star (<span style="color: #ffcc00;">★</span>) <a href="https://github.com/ITAYC0HEN/A-journey-into-Radare2/">the repository</a> to get updates on more radare2 tutorials 🙂</span>

<span style="color: #ff0000;"><strong>Please, be careful when using this sample. It is a real malware, and more than that, a wiper! Use with caution!</strong></span>

_Since we&#8217;ll analyze Dropshot statically, you can use a Linux machine, as I did._

# 

<!--more-->

## <span class="ez-toc-section" id="Getting_Started"></span>Getting Started<span class="ez-toc-section-end"></span>

Assuming you went through the first part of the article, you are already familiar with Cutter and r2pipe. Moreover, you should already have a basic clue of how Dropshot behaves. Open the Dropshot sample in Cutter, execute in Jupyter the r2pipe script we wrote and seek to the \`main\` function using the &#8220;Functions&#8221; widget or the upper search bar.

<div id="attachment_1540" style="width: 697px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/cutter_dropshot_loadnt.png"><img aria-describedby="caption-attachment-1540" data-attachment-id="1540" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_dropshot_loadnt/#main" data-orig-file="http://www.megabeets.net/uploads/cutter_dropshot_loadnt.png" data-orig-size="1920,1056" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter_dropshot_loadnt" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter_dropshot_loadnt-300x165.png" data-large-file="http://www.megabeets.net/uploads/cutter_dropshot_loadnt-1024x563.png" decoding="async" loading="lazy" class="wp-image-1540 size-large" src="https://www.megabeets.net/uploads/cutter_dropshot_loadnt-1024x563.png" alt="" width="687" height="378" srcset="https://www.megabeets.net/uploads/cutter_dropshot_loadnt-1024x563.png 1024w, https://www.megabeets.net/uploads/cutter_dropshot_loadnt-150x83.png 150w, https://www.megabeets.net/uploads/cutter_dropshot_loadnt-300x165.png 300w, https://www.megabeets.net/uploads/cutter_dropshot_loadnt-768x422.png 768w, https://www.megabeets.net/uploads/cutter_dropshot_loadnt-800x440.png 800w, https://www.megabeets.net/uploads/cutter_dropshot_loadnt.png 1920w" sizes="(max-width: 687px) 100vw, 687px" /></a>
  
  <p id="caption-attachment-1540" class="wp-caption-text">
    A function we analyzed in the previous article | Click to enlarge
  </p>
</div>

## <span class="ez-toc-section" id="main"></span>main ()<span class="ez-toc-section-end"></span>

The [role of the `main()` function][9] in a program shouldn&#8217;t be new to you since it is one of the fundamental concepts of programming. Using the Graph mode, we&#8217;ll go thorugh `main`&#8216;s flow in order to find our target &#8211; the resource decryption routine. We can see that a function at `0x403b30` is being called at the first block of `main`.

[<img data-attachment-id="1522" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/dropshot_cutter_main/#main" data-orig-file="http://www.megabeets.net/uploads/dropshot_cutter_main.png" data-orig-size="380,388" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="dropshot_cutter_main" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/dropshot_cutter_main-294x300.png" data-large-file="http://www.megabeets.net/uploads/dropshot_cutter_main.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1522" src="https://www.megabeets.net/uploads/dropshot_cutter_main.png" alt="" width="380" height="388" srcset="https://www.megabeets.net/uploads/dropshot_cutter_main.png 380w, https://www.megabeets.net/uploads/dropshot_cutter_main-147x150.png 147w, https://www.megabeets.net/uploads/dropshot_cutter_main-294x300.png 294w" sizes="(max-width: 380px) 100vw, 380px" />][10]

Double-clicking this line will take us to the graph of `fcn.00403b30`, a rather big function. Going through this function, we&#8217;ll see some non-sense Windows API calls with invalid arguments. When describing Dropshot, I said that it uses anti-emulation heavily &#8211; this function, for example, performs anti-emulation.

<div id="attachment_1523" style="width: 626px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/dropshot_anti_emulation_1.png"><img aria-describedby="caption-attachment-1523" data-attachment-id="1523" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/dropshot_anti_emulation_1/#main" data-orig-file="http://www.megabeets.net/uploads/dropshot_anti_emulation_1.png" data-orig-size="626,584" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="dropshot_anti_emulation_1" data-image-description="" data-image-caption="<p>Click to enlarge</p>
" data-medium-file="http://www.megabeets.net/uploads/dropshot_anti_emulation_1-300x280.png" data-large-file="http://www.megabeets.net/uploads/dropshot_anti_emulation_1.png" decoding="async" loading="lazy" class="size-full wp-image-1523" src="https://www.megabeets.net/uploads/dropshot_anti_emulation_1.png" alt="" width="626" height="584" srcset="https://www.megabeets.net/uploads/dropshot_anti_emulation_1.png 626w, https://www.megabeets.net/uploads/dropshot_anti_emulation_1-150x140.png 150w, https://www.megabeets.net/uploads/dropshot_anti_emulation_1-300x280.png 300w" sizes="(max-width: 626px) 100vw, 626px" /></a>
  
  <p id="caption-attachment-1523" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

### <span class="ez-toc-section" id="Anti-Emulation"></span>Anti-Emulation<span class="ez-toc-section-end"></span>

Anti-emulation techniques are used to fool the emulators of anti-malware products. The emulators are one of the most important components of many security products. Among others, they are used to analyze the behavior of malware, unpack samples and to analyze shellcode. It is doing this by emulating the program&#8217;s workflow by mimicking the target architecture&#8217;s instruction set, as well as the running environment, and dozens or even hundreds of popular API functions. All this is done in order to make a malware ‘think’ it has been executed in a real environment by a victim user.

The emulator engine is mimicking the API or the system calls that are offered by the actual operating systems. Usually, it will implement popular API functions from libraries such as _user32.dll_, _kernel32.dll_, and _ntdll.dll_. Most of the times this will be a dummy implementation where the fake functions won&#8217;t really do anything except returning a successful return value.

<p style="direction: ltr;">
  By using different anti-emulation techniques, malware authors are trying to fool a generic or even a specific emulator. The most common technique, which is also implemented in Dropshot&#8217;s <code>fcn.00403b30</code>, is the use of uncommon or undocumented API calls. This technique can be improved by using incorrect arguments (like <em>NULL</em>) to a certain API function which should cause an Access Violation exception in a real environment.
</p>

In our case, Dropshot is calling some esoteric functions as well as passing non-sense arguments to different API functions.

<span style="font-size: 10pt;"><em>More information about emulation and anti-emulation mechanisms are available in the following, highly recommended, book: <a href="https://www.amazon.com/Antivirus-Hackers-Handbook-Joxean-Koret/dp/1119028752"><span id="productTitle" class="a-size-extra-large">The Antivirus Hacker&#8217;s Handbook</span></a></em></span>

Now that we know all this, we can rename this function from `fcn.00403b30` to a more meaningful name. I used &#8220;AntiEmulation&#8221; but you can choose whatever name you want, as long as it is meaningful to you. Clicking the call instruction and then pressing Shift+N will open the Rename dialog box. Right-clicking the row and choosing &#8220;Rename&#8221; will do the job as well.

[<img data-attachment-id="1493" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/rename_function_antiemulation/#main" data-orig-file="http://www.megabeets.net/uploads/rename_function_AntiEmulation.png" data-orig-size="506,195" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="rename_function_AntiEmulation" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/rename_function_AntiEmulation-300x116.png" data-large-file="http://www.megabeets.net/uploads/rename_function_AntiEmulation.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1493" src="https://www.megabeets.net/uploads/rename_function_AntiEmulation.png" alt="" width="506" height="195" srcset="https://www.megabeets.net/uploads/rename_function_AntiEmulation.png 506w, https://www.megabeets.net/uploads/rename_function_AntiEmulation-150x58.png 150w, https://www.megabeets.net/uploads/rename_function_AntiEmulation-300x116.png 300w" sizes="(max-width: 506px) 100vw, 506px" />][11]After `main` is calling to the AntiEmulation function, we are facing a branch. Here&#8217;s the assembly, copied from Cutter&#8217;s Disassembly widget:

<pre class="lang:sh decode:true">|           0x004041a6           call AntiEmulation
|           0x004041ab           mov eax, 1
|           0x004041b0           test eax, eax
|       ,=&lt; 0x004041b2           je 0x40429d
|       |   0x004041b8           push 4</pre>

As you can see, the code would never branch to `0x40429d` since this `test eax, eax` followed by `je ...` is basically checking whether `eax` equals 0. One instruction before, the program moved the value 1 to `eax`, thus `0x40429d` would be [The Road Not Taken][12].

We&#8217;ll skip the next block which is responsible for creating temporary files and take a look at the block starting at `0x4041f9`. In this block, we&#8217;ll see that Dropshot is creating a modeless Dialog box using [`CreateDialogParamA`][13] with the following parameters: `CreateDialogParamA(0, 0x410, 0, DialogFunc, 0);`.

[<img data-attachment-id="1494" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/main_createdialog/#main" data-orig-file="http://www.megabeets.net/uploads/main_CreateDialog.png" data-orig-size="469,361" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="main_CreateDialog" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/main_CreateDialog-300x231.png" data-large-file="http://www.megabeets.net/uploads/main_CreateDialog.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1494" src="https://www.megabeets.net/uploads/main_CreateDialog.png" alt="" width="469" height="361" srcset="https://www.megabeets.net/uploads/main_CreateDialog.png 469w, https://www.megabeets.net/uploads/main_CreateDialog-150x115.png 150w, https://www.megabeets.net/uploads/main_CreateDialog-300x231.png 300w" sizes="(max-width: 469px) 100vw, 469px" />][14]

The DialogPrc callback which is passed to `CreateDialogParamA` is recognized by radare2 and shown by Cutter as `fcn.DialogFunc`. This function contains the main logic of the dropper and this is the function that we&#8217;ll focus on. Later in this block, `ShowWindow` is being called in order to &#8220;show&#8221; the window. Obviously, this is a dummy window which would never be shown since the malware author doesn&#8217;t want any artifact to be shown to the victim. `ShowWindow` will trigger the execution of `fcn.DialogFunc`.

Double-clikcing on `fcn.DialogFunc` will take us to the function itself. We can see that it is performing several comparisons for the messages it receives and then is calling to a very interesting function `fcn.00403240`.

## 

## <span class="ez-toc-section" id="Handling_the_Resources"></span>Handling  the Resources<span class="ez-toc-section-end"></span>

The first block of `fcn.00403240` is pretty straightforward. Dropshot is getting a handle to itself using `GetModuleHandleA`. Then, by using `FindResourceA`, it is locating a resource with a dummy type 0x67 (Decimal: 111) and a name 0x6e (Decimal: 110). Finally, it is loading a resource with this name using `LoadResource`.

[<img data-attachment-id="1495" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/first_bb_load_dummy_rsrc/#main" data-orig-file="http://www.megabeets.net/uploads/first_bb_load_dummy_rsrc.png" data-orig-size="468,542" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="first_bb_load_dummy_rsrc" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/first_bb_load_dummy_rsrc-259x300.png" data-large-file="http://www.megabeets.net/uploads/first_bb_load_dummy_rsrc.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1495" src="https://www.megabeets.net/uploads/first_bb_load_dummy_rsrc.png" alt="" width="468" height="542" srcset="https://www.megabeets.net/uploads/first_bb_load_dummy_rsrc.png 468w, https://www.megabeets.net/uploads/first_bb_load_dummy_rsrc-130x150.png 130w, https://www.megabeets.net/uploads/first_bb_load_dummy_rsrc-259x300.png 259w" sizes="(max-width: 468px) 100vw, 468px" />][15]

Using Cutter, we can see the content of this resource. Simply go to the Resources widget and locate the resource with the name &#8220;110&#8221;.

[<img data-attachment-id="1496" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_resources_window1/#main" data-orig-file="http://www.megabeets.net/uploads/Cutter_Resources_window1.png" data-orig-size="640,365" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="Cutter_Resources_window1" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/Cutter_Resources_window1-300x171.png" data-large-file="http://www.megabeets.net/uploads/Cutter_Resources_window1.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1496" src="https://www.megabeets.net/uploads/Cutter_Resources_window1.png" alt="" width="640" height="365" srcset="https://www.megabeets.net/uploads/Cutter_Resources_window1.png 640w, https://www.megabeets.net/uploads/Cutter_Resources_window1-150x86.png 150w, https://www.megabeets.net/uploads/Cutter_Resources_window1-300x171.png 300w" sizes="(max-width: 640px) 100vw, 640px" />][16]As you can see in the screenshot above, the size of the resource is 28 Bytes and its Lang is Farsi which might hint us about the threat actor behind Dropshot.

Double-clicking the resource will take us to the resource&#8217;s address. Let&#8217;s click on the Hexdump widget to see its data. In the hexdump we can see that this resource contains the following bytes: `01 00 00 00 00 00 00 00 01 00 00 00 00 00 00 00 01 00 00 00 01 00 00 00 00 00 00 00`. Those of you who are familiar with radare2 may use the Console widget in the bottom left to do this quickly with `px`:

[<img data-attachment-id="1497" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_console_px28/#main" data-orig-file="http://www.megabeets.net/uploads/cutter_console_px28.png" data-orig-size="700,282" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter_console_px28" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter_console_px28-300x121.png" data-large-file="http://www.megabeets.net/uploads/cutter_console_px28.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1497" src="https://www.megabeets.net/uploads/cutter_console_px28.png" alt="" width="700" height="282" srcset="https://www.megabeets.net/uploads/cutter_console_px28.png 700w, https://www.megabeets.net/uploads/cutter_console_px28-150x60.png 150w, https://www.megabeets.net/uploads/cutter_console_px28-300x121.png 300w" sizes="(max-width: 700px) 100vw, 700px" />][17]This resource will be used later but we won&#8217;t be getting into it since it is out of the scope of this post.

After loading the resource, in the next block we can see the start of a loop:

[<img data-attachment-id="1498" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_dummy_loop_lod/#main" data-orig-file="http://www.megabeets.net/uploads/cutter_dummy_loop_lod.png" data-orig-size="538,279" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter_dummy_loop_lod" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter_dummy_loop_lod-300x156.png" data-large-file="http://www.megabeets.net/uploads/cutter_dummy_loop_lod.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1498" src="https://www.megabeets.net/uploads/cutter_dummy_loop_lod.png" alt="" width="538" height="279" srcset="https://www.megabeets.net/uploads/cutter_dummy_loop_lod.png 538w, https://www.megabeets.net/uploads/cutter_dummy_loop_lod-150x78.png 150w, https://www.megabeets.net/uploads/cutter_dummy_loop_lod-300x156.png 300w" sizes="(max-width: 538px) 100vw, 538px" />][18]This loop is checking if `local_2ch` equals to 0x270f (Decimal: 9999) and if yes it exits from the loop. Inside this loop, there will be another loop of 999 iterations. So basically, this is how this nested loop looks like:

<pre class="lang:c decode:true">for ( i = 0; i &lt; 9999; ++i )
  {
    for ( j = 0; j &lt; 999; ++j )
    {
        dummy_code;
    }
  }</pre>

<p style="direction: ltr;">
  This is just another anti-emulation\analysis technique which is basically doing nothing. This is another example of the heavy use of anti-emulation by Dropshot.
</p>

After this loop, the right branch is taken and this is an interesting one.

<div id="attachment_1499" style="width: 500px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/tdecrypt_2ndbranch.png"><img aria-describedby="caption-attachment-1499" data-attachment-id="1499" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/tdecrypt_2ndbranch/#main" data-orig-file="http://www.megabeets.net/uploads/tdecrypt_2ndbranch.png" data-orig-size="500,825" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="tdecrypt_2ndbranch" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/tdecrypt_2ndbranch-182x300.png" data-large-file="http://www.megabeets.net/uploads/tdecrypt_2ndbranch.png" decoding="async" loading="lazy" class="wp-image-1499 size-full" src="https://www.megabeets.net/uploads/tdecrypt_2ndbranch.png" alt="" width="500" height="825" srcset="https://www.megabeets.net/uploads/tdecrypt_2ndbranch.png 500w, https://www.megabeets.net/uploads/tdecrypt_2ndbranch-91x150.png 91w, https://www.megabeets.net/uploads/tdecrypt_2ndbranch-182x300.png 182w" sizes="(max-width: 500px) 100vw, 500px" /></a>
  
  <p id="caption-attachment-1499" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

At first, `VirutalAlloc` in the size of 512 bytes is being called. Next, `fcn.00401560` is called with 3 arguments. Let&#8217;s enter this function and see what&#8217;s in there:

<div id="attachment_1500" style="width: 912px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/fcn00401560.png"><img aria-describedby="caption-attachment-1500" data-attachment-id="1500" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/fcn00401560/#main" data-orig-file="http://www.megabeets.net/uploads/fcn00401560.png" data-orig-size="902,567" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="fcn00401560" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/fcn00401560-300x189.png" data-large-file="http://www.megabeets.net/uploads/fcn00401560.png" decoding="async" loading="lazy" class="wp-image-1500 size-full" src="https://www.megabeets.net/uploads/fcn00401560.png" alt="" width="902" height="567" srcset="https://www.megabeets.net/uploads/fcn00401560.png 902w, https://www.megabeets.net/uploads/fcn00401560-150x94.png 150w, https://www.megabeets.net/uploads/fcn00401560-300x189.png 300w, https://www.megabeets.net/uploads/fcn00401560-768x483.png 768w, https://www.megabeets.net/uploads/fcn00401560-800x503.png 800w" sizes="(max-width: 902px) 100vw, 902px" /></a>
  
  <p id="caption-attachment-1500" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

Hey! Look who&#8217;s back! We can see the 2 functions we analyzed in the previous article: `decryption_function` and `load_ntdll_or_kernel32`. That&#8217;s great! Also, you can notice the comment on `call decryption_function` which is telling us that the decrypted string is `GetModuleFileNameW`. This comment is the result of the r2pipe script we wrote.

In this function, `GetModuleFileNameW` will be decrypted, then Kernel32.dll will be loaded and `GetProcAddress` will be called to get the address of `GetModuleFileNameW` and then it will move it to `[0x41dc04]`. Later in this function, `[0x41dc04]` will be called.

Basically, this function is wrapper around `GetModuleFileNameW`, something which is common in Dropshot&#8217;s code. Let&#8217;s rename this function to `w_GetModuleFileNameW` where &#8220;w_&#8221; stands for &#8220;wrapper&#8221;. Of course, you can choose whatever naming convention you prefer.

Right after the call to `w_GetModuleFileNameW`, Dropshot is using VirtualAlloc to allocate 20 (0x14) bytes, then there is a call to `fcn.00401a40` which is a function quite similar to [`memset`][19], it is given with 3 arguments (_address, value_ and _size_), just like `memset` and it is responsible to fill the range from `address` to `address+size` with the given `value`. Usually, along the program, this function is used to fill an allocated buffer with zeroes. This is quite strange to me, since VirtualAlloc is already &#8220;initializes the memory it allocates to zero&#8221;. Let&#8217;s name this function to `memset_` using Shift+N or via right click and move on.

Right after the program is zeroing-out the allocated memory, we see a call to another function &#8211; `fcn.00401c90`. We can see 3 arguments which are being passed to it, 0x14, 0x66, and 0x68. Since sometimes we prefer to see decimal numbers and not hex, let&#8217;s use another useful trick of Cutter. Right-click on any of these hex numbers and choose &#8220;Set Immediate Base to&#8230;&#8221; and then select &#8220;Decimal&#8221;.

[<img data-attachment-id="1502" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_set_imm_base/#main" data-orig-file="http://www.megabeets.net/uploads/cutter_set_imm_base.png" data-orig-size="787,464" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter_set_imm_base" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter_set_imm_base-300x177.png" data-large-file="http://www.megabeets.net/uploads/cutter_set_imm_base.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1502" src="https://www.megabeets.net/uploads/cutter_set_imm_base.png" alt="" width="787" height="464" srcset="https://www.megabeets.net/uploads/cutter_set_imm_base.png 787w, https://www.megabeets.net/uploads/cutter_set_imm_base-150x88.png 150w, https://www.megabeets.net/uploads/cutter_set_imm_base-300x177.png 300w, https://www.megabeets.net/uploads/cutter_set_imm_base-768x453.png 768w" sizes="(max-width: 787px) 100vw, 787px" />][20]Now we can see that the values which are being passed to `fcn.00401c90` are 20, 102 and 104. Looks familiar? 20 was the size of the buffer that was just allocated. 102 and 104 remind us the Resource name and type that used before (110 and 111). Are we dealing with resources here? Let&#8217;s see.

Moving to the Resources widget again, we can see that there&#8217;s indeed a resource named &#8220;102&#8221; which &#8220;104&#8221; is its type. And yes, it is 19B long, close enough 😉

`fcn.00401c90`is one of the key functions involved in the dropper functionality of Dropshot. The thing is, that this function is rather big and quite complicated when you don&#8217;t know how to look at it. We&#8217;ll get back to it in one minute but before that, I want to show you an approach I use while reverse engineering some pieces of code and while facing a chain of calls to functions which are probably related to each other.

First, we saw that 20 bytes were allocated by `VirtualAlloc` and the pointer to the allocated memory was moved to `[local_ch]`. Right after that, `memset_` was called in order to zero-out 20 bytes at`[local_ch]`, i.e to zero-out the allocated buffer. Immediately after, `fcn.00401c90` was called and 3 arguments were passed to it &#8211; 104, 102 and our beloved 20. We know that 102 is a name of a resource and its size is almost 20. We don&#8217;t know yet what this function is doing but we know that its return value(`eax`) is being passed along with 2 more arguments to another function, `fcn.00401a80`. The other arguments are, you guessed right, 20 and the allocated buffer. A quick look at `fcn.00401a80`, which is a really tiny function, will reveal us that this function is copying a buffer to the allocated memory. This function is quite similar to [`memcpy`][21] so we&#8217;ll rename it to `memcpy_`. So now we can do an educated guess and say that `fcn.00401c90` is reading a resource to a buffer and returns a pointer to it.

Using this approach, we can understand (or at least guess) a complicated function without even analyzing it. Just by looking at a programs chain of function calls, we can build the puzzle and save us important time.

That said, we&#8217;ll still give this function a quick analysis because we want to be sure that we guessed right, and more importantly &#8212; because this is an interesting function.

## <span class="ez-toc-section" id="Resource_Parser"></span>Resource Parser<span class="ez-toc-section-end"></span>

The next part is where things are getting more complicated. We&#8217;ll start by going over `fcn.00401c90` pretty fast so try to follow. Also, you may want to make sure you fasten yourself since we are going on a rollercoaster ride through the PE structure.

Take a look at the first block of this function. You&#8217;ll see one `call` and a lot of `mov`, `add` and calculation of offsets. This is how a typical PE parsing looks like.

<pre class="lang:asm decode:true">/ (fcn) fcn.00401c90 468
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
|       ,=&lt; 0x00401d15           jmp 0x401d20
...
...</pre>

At first, a handle to the current process is received using `GetModuleHandleW`. Then, the handle (`eax`) is being moved to a variety of local variables. First, it is being moved to `[local_34h]` at `0x00401ca5`. Then you can see `eax` moved to `[local_20h]` which is later being moved to `[local_24h]` using `ecx`.

So basically we have a bunch of local variables that currently hold the handle `hmodule`. We can rename all three variables to `[hmodule_x]` so it&#8217;ll be easier to keep track of all the reference to `hmodule`. To rename flags you can use the Console widget and just execute `afvn old_name new_name`. For example, I executed: `afvn local_34h hmodule_1; afvn local_20h hmodule_2; afvn local_24h hmodule_3`.

`GetModuleHandle` returns a handle to a mapped module, this basically means that our `hmodule`s point to our binary&#8217;s base address. In line `0x00401cb4` we can see that `[hmodule_3]` is moved to `edx`, then the value at `[edx + 0x3c]` is being moved to `eax` and `[hmodule_3]` is added to it at `0x00401cba`. Finally, `eax` is moved to `[local_38h]`. To put it simply, we can use the following pseudo-code:

<pre class="lang:c decode:true">[local_38h] = (BYTE*)hmodule + *(hmodule + 0x3c)</pre>

So what&#8217;s in this address? Use your favorite binary structure viewer to find out. In this example, I&#8217;ll use [PEview][22] but you can use any other program you prefer &#8211; including the binary structure parsing feature of radare2, if you&#8217;re already a radare2 pro (see [`pf?`][23]).

Open Dropshot in PEview and inspect the DOS Header:

[<img data-attachment-id="1505" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter-peview_dos_header/#main" data-orig-file="http://www.megabeets.net/uploads/cutter-peview_dos_header.png" data-orig-size="657,395" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter-peview_dos_header" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter-peview_dos_header-300x180.png" data-large-file="http://www.megabeets.net/uploads/cutter-peview_dos_header.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1505" src="https://www.megabeets.net/uploads/cutter-peview_dos_header.png" alt="" width="657" height="395" srcset="https://www.megabeets.net/uploads/cutter-peview_dos_header.png 657w, https://www.megabeets.net/uploads/cutter-peview_dos_header-150x90.png 150w, https://www.megabeets.net/uploads/cutter-peview_dos_header-300x180.png 300w" sizes="(max-width: 657px) 100vw, 657px" />][24]As you can see, in offset `0x3c` there is a pointer (0x108) to the offset to the new EXE Header which is basically the [IMAGE\_NT\_HEADER][25]. Awesome! So `[local_38h]` holds the address of the NT Header. Let&#8217;s rename it to `NT_HEADER` and move on.

At address `0x00401cc0` we can see that `NT_HEADER` is moved to `ecx` and then the program is adding 0x18 to `ecx`. Last, the value in `ecx` is moved to `[local_3ch]`. Just as before, let&#8217;s open again our PE parser and check what is in `NT_HEADER + 0x18`. Adding 0x18 to 0x108 will give us 0x120. Let&#8217;s see what is in this offset:

<div id="attachment_1506" style="width: 945px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/cutter-peview_nt_header.png" target="_blank" rel="noopener noreferrer"><img aria-describedby="caption-attachment-1506" data-attachment-id="1506" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter-peview_nt_header/#main" data-orig-file="http://www.megabeets.net/uploads/cutter-peview_nt_header.png" data-orig-size="935,395" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter-peview_nt_header" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter-peview_nt_header-300x127.png" data-large-file="http://www.megabeets.net/uploads/cutter-peview_nt_header.png" decoding="async" loading="lazy" class="wp-image-1506 size-full" src="https://www.megabeets.net/uploads/cutter-peview_nt_header.png" alt="" width="935" height="395" srcset="https://www.megabeets.net/uploads/cutter-peview_nt_header.png 935w, https://www.megabeets.net/uploads/cutter-peview_nt_header-150x63.png 150w, https://www.megabeets.net/uploads/cutter-peview_nt_header-300x127.png 300w, https://www.megabeets.net/uploads/cutter-peview_nt_header-768x324.png 768w, https://www.megabeets.net/uploads/cutter-peview_nt_header-800x338.png 800w" sizes="(max-width: 935px) 100vw, 935px" /></a>
  
  <p id="caption-attachment-1506" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

Nice! 0x120 is the offset of the [IMAGE\_OPTIONAL\_HEADER][26] as can be seen in the image above. Let&#8217;s rename `local_3ch` to `OPTIONAL_HEADER`. To cut a _long story short_, Dropshot is then parsing the [IMAGE\_DATA\_DIRECTORY][27] structure (OPTIONAL\_HEADER + 0x60), the RESOURCE\_TABLE, and last it iterates through the different resources and compares the resource type and the resource name to the function&#8217;s arguments. Finally, it uses `memcpy_` to copy the content of the required resource to a variable and returns this variable.

Now we can rename the function to `get_resource` and the arguments to the corresponding meaning of them by executing `afvn arg_8h arg_rsrc_type; afvn arg_ch arg_rsrc_name; afvn arg_10h arg_dwsize`.

Now that we are sure about what this function does, we can see where else it is referenced. Right-click on the function and choosing &#8220;Show X-Refs&#8221; (or simply pressing &#8216;x&#8217;) will take us to the X-Refs window. We can see that `get_resource` is being called from two locations. One (`0x0040336d`) is already familiar to us, it is called to get resource &#8220;102&#8221;. The second call (at `0x00403439`), a few instructions later, is new to us &#8212; it is called to get the content of another resource, named &#8220;101&#8221; (0x65).

[<img data-attachment-id="1507" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/x-refs_to_get_resource/#main" data-orig-file="http://www.megabeets.net/uploads/x-refs_to_get_resource.png" data-orig-size="746,407" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="x-refs_to_get_resource" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/x-refs_to_get_resource-300x164.png" data-large-file="http://www.megabeets.net/uploads/x-refs_to_get_resource.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1507" src="https://www.megabeets.net/uploads/x-refs_to_get_resource.png" alt="" width="746" height="407" srcset="https://www.megabeets.net/uploads/x-refs_to_get_resource.png 746w, https://www.megabeets.net/uploads/x-refs_to_get_resource-150x82.png 150w, https://www.megabeets.net/uploads/x-refs_to_get_resource-300x164.png 300w" sizes="(max-width: 746px) 100vw, 746px" />][28]

Remember the screenshot of the Resources widget from before? We can see there the resource named &#8220;101&#8221;. What makes &#8220;101&#8221; so interesting is that it is **much** bigger than the other &#8212; its size is 69.6 KB! This is Dropshot&#8217;s payload. By going to the Resources widget and double-clicking &#8220;101&#8221; will take us to the resource&#8217;s offset in the binary. In the Hexdump widget, we can see that the content of this resource makes no sense and has a really high entropy (7.8 out of the maximum 8):

<div id="attachment_1509" style="width: 697px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/resource_101_content_and_entropy.png"><img aria-describedby="caption-attachment-1509" data-attachment-id="1509" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/resource_101_content_and_entropy/#main" data-orig-file="http://www.megabeets.net/uploads/resource_101_content_and_entropy.png" data-orig-size="1149,628" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="resource_101_content_and_entropy" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/resource_101_content_and_entropy-300x164.png" data-large-file="http://www.megabeets.net/uploads/resource_101_content_and_entropy-1024x560.png" decoding="async" loading="lazy" class="wp-image-1509 size-large" src="https://www.megabeets.net/uploads/resource_101_content_and_entropy-1024x560.png" alt="" width="687" height="376" srcset="https://www.megabeets.net/uploads/resource_101_content_and_entropy-1024x560.png 1024w, https://www.megabeets.net/uploads/resource_101_content_and_entropy-150x82.png 150w, https://www.megabeets.net/uploads/resource_101_content_and_entropy-300x164.png 300w, https://www.megabeets.net/uploads/resource_101_content_and_entropy-768x420.png 768w, https://www.megabeets.net/uploads/resource_101_content_and_entropy-800x437.png 800w, https://www.megabeets.net/uploads/resource_101_content_and_entropy.png 1149w" sizes="(max-width: 687px) 100vw, 687px" /></a>
  
  <p id="caption-attachment-1509" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

This data is compressed/encrypted somehow so we need to decrypt it. Let&#8217;s continue our analysis to find out how.

## <span class="ez-toc-section" id="How_To_Decrypt_The_Resource"></span>How To Decrypt The Resource<span class="ez-toc-section-end"></span>

In order to decrypt the resource, we should follow the program&#8217;s flow to see how and where the payload is being used. Right after `get_resource` is being called with &#8220;101&#8221; and &#8220;103&#8221;, the resource is copied to `[local_20h]` using `memcpy_` (at `0x00403446`). Let&#8217;s call it `compressed_payload`. The compressed buffer is then passed to `fcn.00401e70` which is a function that performs dummy math calculations on the resource&#8217;s data. Probably another Anti-Emulation technique or simply a way to waste our time. I&#8217;ll rename it to `dummy_math`. Next, `compressed_payload` is being passed to `fcn.00401ef0` along with another buffer `[local_54h]`.

The analysis of this function is out of the scope of this article but this function is responsible to decompress a buffer using [zlib][29] and put the decompressed buffer in `[local_54h]`. You can see, for example, that `fcn.00401ef0` is calling to `fcn.004072f0` which contains strings like &#8220;unknown compression method&#8221; and &#8220;invalid window size&#8221; which can be found in the file [inflate.c][30] in the zlib repository. I renamed `fcn.00401ef0` to `zlib_decompress` and `local_54h` to `decompressed_payload`.

I&#8217;ll tell you now that simply a decompression of the buffer isn&#8217;t enough since there&#8217;s still another simple decryption to do. Straight after the decompressing, we can see more of the Anti-Emulation which we are already familiar with. Finally, our decompressed buffer is being passed to `fcn.00402620`. This function is responsible for the last decryption of the resource and then it performs a notorious technique known as [&#8220;Process Hollowing&#8221;][31] or &#8220;RunPE&#8221; in order to execute the decrypted payload.

So how `fcn.00402620` decrypts the decompressed payload? Simply, it uses `ror 3` to rotate-right each byte in the decompressed buffer. 3 stands for the number of bits to rotate.

[<img data-attachment-id="1512" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/ror3_dropshot/#main" data-orig-file="http://www.megabeets.net/uploads/ror3_dropshot.png" data-orig-size="468,408" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="ror3_dropshot" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/ror3_dropshot-300x262.png" data-large-file="http://www.megabeets.net/uploads/ror3_dropshot.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1512" src="https://www.megabeets.net/uploads/ror3_dropshot.png" alt="" width="468" height="408" srcset="https://www.megabeets.net/uploads/ror3_dropshot.png 468w, https://www.megabeets.net/uploads/ror3_dropshot-150x131.png 150w, https://www.megabeets.net/uploads/ror3_dropshot-300x262.png 300w" sizes="(max-width: 468px) 100vw, 468px" />][32]

The rest of this function is interesting as well but it has nothing to do with decrypting the resource so I&#8217;ll leave it to you.

To sum things up, and before we adding the logic for the resource decryption inside the script we wrote in the previous article &#8211; let&#8217;s sketch how the decryption function should look like. It should be something like this:

<pre class="lang:python decode:true ">rsrc_101 = get_resource("101")

decompressed_payload = decompress(rsrc_101)

decrypted_payload = []

for b in decompressed_payload:
   decrypted_payload.append(ror3(b))</pre>

## <span class="ez-toc-section" id="Scripting_time_Decrypting_the_resource"></span>Scripting time! Decrypting the resource<span class="ez-toc-section-end"></span>

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

<pre class="lang:python decode:true ">rsrcs = cutter.cmdj('iRj')
rsrc_101 = {}

# Locate resource 101 and dump it to an array
for rsrc in rsrcs:
    if rsrc['name'] == 101:
        rsrc_101 = cutter.cmdj("pxj %d @ %d" %
                                (rsrc['size'], rsrc['vaddr']))</pre>

> `iR` was used to get the list of resources and their offsets in the file. Next, we iterate through the different resources untill we find a resource named &#8220;101&#8221;. Last, using `px` we are reading the resource&#8217;s bytes into a varibale. We appended `j` to the commands in order to get their output as JSON.

Next, we want to decompress the buffer using zlib. Python is coming with &#8220;zlib&#8221; library by default which is great news for us. Add `import zlib` to the top of the script and use this code to decompress the buffer:

<pre class="lang:python decode:true"># Decompress the zlibbed array
decompressed_data = zlib.decompress(bytes(rsrc_101))</pre>

Now that our buffer is decompressed in `decompressed_data`, all we left to do is to perform right rotation on the data and save it to a file.

Define [the following][38] `ror` lambda:

<pre class="lang:python decode:true">def ror(val, r_bits, max_bits): return \
    ((val & (2**max_bits-1)) &gt;&gt; r_bits % max_bits) | \
    (val &lt;&lt; (max_bits-(r_bits % max_bits)) & (2**max_bits-1))</pre>

And use it in your code like this:

<pre class="lang:default decode:true">decrypted_payload = []

# Decrypt the payload
for b in decompressed_data:
    decrypted_payload.append((ror(b, 3, 8)))</pre>

Last, save it to a file:

<pre class="lang:python decode:true"># Write the payload (a PE binary) to a file
open(r'./decrypted_rsrc.bin', 'wb').write(bytearray(decrypted_payload))</pre>

Now let&#8217;s combine the script from the previous article to the one we created now and test it in Jupyter. The combined script should first decode the encrypted scripts, and then it should decrypt the resource and save it to the disk.

<p style="text-align: center;">
  <span style="color: #79de5b;"><strong>The final script can be found <a href="https://github.com/ITAYC0HEN/A-journey-into-Radare2/blob/master/Part%203%20-%20Malware%20analysis/decrypt_dropshot.py"><span style="font-size: 12pt;">here</span></a></strong></span>.
</p>

Copy it and paste it into your Jupyter notebook. You can also execute your version of the code to see if you got it right by yourself.

[<img data-attachment-id="1518" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/jupyter_final_output/#main" data-orig-file="http://www.megabeets.net/uploads/jupyter_final_output.png" data-orig-size="601,866" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="jupyter_final_output" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/jupyter_final_output-208x300.png" data-large-file="http://www.megabeets.net/uploads/jupyter_final_output.png" decoding="async" loading="lazy" class="aligncenter size-full wp-image-1518" src="https://www.megabeets.net/uploads/jupyter_final_output.png" alt="" width="601" height="866" srcset="https://www.megabeets.net/uploads/jupyter_final_output.png 601w, https://www.megabeets.net/uploads/jupyter_final_output-104x150.png 104w, https://www.megabeets.net/uploads/jupyter_final_output-208x300.png 208w" sizes="(max-width: 601px) 100vw, 601px" />][39]

Seems like our script was executed successfully and &#8220;Saved the PE to ./decrypted_rsrc.bin&#8221;. Great!

The last thing we want to do is to open `decrypted_rsrc.bin` in a new instance of Cutter in order to verify that this is indeed a PE file and that we didn&#8217;t corrupt the file in some way.

<div id="attachment_1520" style="width: 889px" class="wp-caption aligncenter">
  <a href="https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload.png"><img aria-describedby="caption-attachment-1520" data-attachment-id="1520" data-permalink="https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-2/cutter_dashboard_dropped_payload/#main" data-orig-file="http://www.megabeets.net/uploads/cutter_dashboard_dropped_payload.png" data-orig-size="879,613" data-comments-opened="1" data-image-meta="{&quot;aperture&quot;:&quot;0&quot;,&quot;credit&quot;:&quot;&quot;,&quot;camera&quot;:&quot;&quot;,&quot;caption&quot;:&quot;&quot;,&quot;created_timestamp&quot;:&quot;0&quot;,&quot;copyright&quot;:&quot;&quot;,&quot;focal_length&quot;:&quot;0&quot;,&quot;iso&quot;:&quot;0&quot;,&quot;shutter_speed&quot;:&quot;0&quot;,&quot;title&quot;:&quot;&quot;,&quot;orientation&quot;:&quot;0&quot;}" data-image-title="cutter_dashboard_dropped_payload" data-image-description="" data-image-caption="" data-medium-file="http://www.megabeets.net/uploads/cutter_dashboard_dropped_payload-300x209.png" data-large-file="http://www.megabeets.net/uploads/cutter_dashboard_dropped_payload.png" decoding="async" loading="lazy" class="wp-image-1520 size-full" src="https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload.png" alt="" width="879" height="613" srcset="https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload.png 879w, https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload-150x105.png 150w, https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload-300x209.png 300w, https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload-768x536.png 768w, https://www.megabeets.net/uploads/cutter_dashboard_dropped_payload-800x558.png 800w" sizes="(max-width: 879px) 100vw, 879px" /></a>
  
  <p id="caption-attachment-1520" class="wp-caption-text">
    Click to enlarge
  </p>
</div>

Awesome! Cutter recognized the file as PE and seems like the code is correctly interpreted. This binary we just decrypted and saved is the Wiper module of Dropshot &#8211; a quite interesting piece of malware on its own. This module, just as its dropper, is using heavy anti-emulation and similar technique to decrypt its strings. You can give it a try and analyze it on your own using Cutter, radare2, and r2pipe. Good Luck!

## <span class="ez-toc-section" id="Epilogue"></span>Epilogue<span class="ez-toc-section-end"></span>

Here comes to an end the second and the last part of this series about decrypting Dropshot with Cutter and r2pipe. We got familiar with Cutter, radare2 GUI, and wrote a decryption script in r2pipe’s Python binding. We also analyzed some components of APT33’s Dropshot, an advanced malware.

As always, please post comments to this post or message me [privately][40] if something is wrong, not accurate, needs further explanation or you simply don’t get it. Don’t hesitate to share your thoughts with me.

**Subscribe on the left if you want to get the next articles straight in your inbox.**

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://www.megabeets.net/decrypting-dropshot-with-radare2-and-cutter-part-1/
 [2]: https://www.megabeets.net/uploads/cutter_logo_trimmed.png
 [3]: https://github.com/radareorg/cutter/releases
 [4]: https://github.com/radareorg/cutter/blob/master/docs/Compiling.md
 [5]: https://en.wikipedia.org/wiki/Shamoon
 [6]: https://app.box.com/s/olc867zxc9nkjzm3wkjwi0b0e2awahtn
 [7]: https://www.fireeye.com/blog/threat-research/2017/09/apt33-insights-into-iranian-cyber-espionage.html
 [8]: https://github.com/ITAYC0HEN/A-journey-into-Radare2/blob/master/Part%203%20-%20Malware%20analysis/dropshot.exe.zip
 [9]: http://en.cppreference.com/w/cpp/language/main_function
 [10]: https://www.megabeets.net/uploads/dropshot_cutter_main.png
 [11]: https://www.megabeets.net/uploads/rename_function_AntiEmulation.png
 [12]: https://www.poetryfoundation.org/poems/44272/the-road-not-taken
 [13]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms645445(v=vs.85).aspx
 [14]: https://www.megabeets.net/uploads/main_CreateDialog.png
 [15]: https://www.megabeets.net/uploads/first_bb_load_dummy_rsrc.png
 [16]: https://www.megabeets.net/uploads/Cutter_Resources_window1.png
 [17]: https://www.megabeets.net/uploads/cutter_console_px28.png
 [18]: https://www.megabeets.net/uploads/cutter_dummy_loop_lod.png
 [19]: http://www.cplusplus.com/reference/cstring/memset/
 [20]: https://www.megabeets.net/uploads/cutter_set_imm_base.png
 [21]: http://en.cppreference.com/w/c/string/byte/memcpy
 [22]: http://wjradburn.com/software/
 [23]: http://radare.today/posts/parsing-a-fileformat-with-radare2/
 [24]: https://www.megabeets.net/uploads/cutter-peview_dos_header.png
 [25]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms680336(v=vs.85).aspx
 [26]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms680339(v=vs.85).aspx
 [27]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms680305(v=vs.85).aspx
 [28]: https://www.megabeets.net/uploads/x-refs_to_get_resource.png
 [29]: https://zlib.net/
 [30]: https://github.com/madler/zlib/blob/master/inflate.c#L622
 [31]: https://attack.mitre.org/wiki/Technique/T1093
 [32]: https://www.megabeets.net/uploads/ror3_dropshot.png
 [33]: https://github.com/radare/radare2-r2pipe
 [34]: https://github.com/radare/radare2-r2pipe/tree/master/python
 [35]: https://github.com/radare/radare2-r2pipe/tree/master/nodejs/r2pipe
 [36]: https://github.com/radare/radare2-r2pipe/tree/master/rust
 [37]: https://github.com/radare/radare2-r2pipe/tree/master/c
 [38]: https://www.falatic.com/index.php/108/python-and-bitwise-rotation
 [39]: https://www.megabeets.net/uploads/jupyter_final_output.png
 [40]: https://www.megabeets.net/about.html#contact