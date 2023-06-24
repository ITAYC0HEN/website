---
title: '[H4CK1T 2016] ch17ch47 – Germany Writeup'
author: Megabeets
type: posts
date: 2016-10-03T00:30:10+00:00

categories:
  - H4CK1T CTF 2016
  - Writeups
tags:
  - CTF
  - H4CK1T
  - Writeups

---
### **Description:**

> **ch17ch47 &#8211; Germany &#8211; 200 &#8211; Forensics**  
> <span style="font-weight: 400;">Find out who is the recipient of the information from the agent.</span>  
> [<span style="font-weight: 400;">http://ctf.com.ua/data/attachments/CorpUser.zip</span>][1]

This challenge was [second][2] in this CTF which took me no more then five simple and basic commands in order to get the flag.

I roughly follow the same simple system whenever I face a new challenge. This system has prove itself again and again in almost any kind of challenge in different levels.

<div class="im_history_message_wrap im_message_focus">
  <div class="im_message_outer_wrap">
    <div class="im_message_wrap clearfix">
      <div class="im_content_message_wrap im_message_out">
        <div class="im_message_body">
          <ol>
            <li class="im_message_text" dir="auto">
              Examine the file types that are given to you: An image, pcap, pe, etc. You can do it using the <code>file</code> command or just by open it
            </li>
            <li class="im_message_text" dir="auto">
              Run &#8216;strings&#8217; command on it. ```sh
strings file_name | grep - i flag{convention}
```

            </li>
            
            <li class="im_message_text" dir="auto">
              Run foremost (and binwalk) on the file
            </li>
            <li class="im_message_text" dir="auto">
              Run <code>strings</code> on all the extracted files
            </li>
          </ol>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="im_history_message_wrap im_grouped_short">
</div>

<div class="im_history_message_wrap im_grouped_short">
</div>

<div class="im_history_message_wrap im_grouped_short">
  This time we are given with a zip file. First, we want to unzip it in order to examine the files inside. It has a lot of file so I don&#8217;t paste here the full output.
</div>

<div class="im_history_message_wrap im_grouped_short">
  ```sh
Megabeets:/tmp/h4ckit/germany# unzip CorpUser.zip
Archive:  CorpUser.zip
   creating: CorpUser/
   creating: CorpUser/AppData/
   creating: CorpUser/AppData/Local/
   creating: CorpUser/AppData/Local/Apps/
   creating: CorpUser/AppData/Local/Apps/2.0/
   creating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/
   creating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/
   creating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/clic...exe_baa8013a79450f71_0001.0003_none_8554920337a51673/
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/clic...exe_baa8013a79450f71_0001.0003_none_8554920337a51673/GoogleUpdateSetup.exe
   creating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/GoogleUpdateSetup.exe
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/clickonce_bootstrap.exe
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/clickonce_bootstrap.exe.cdf-ms
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/clickonce_bootstrap.exe.manifest
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/clickonce_bootstrap_unsigned.cdf-ms
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/google.app_baa8013a79450f71_0001.0003_75c9b16f02ab5371/clickonce_bootstrap_unsigned.manifest
   creating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/manifests/
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/manifests/clic...exe_baa8013a79450f71_0001.0003_none_8554920337a51673.cdf-ms
  inflating: CorpUser/AppData/Local/Apps/2.0/2VQLGYNY.ZHM/X90XE08W.MG4/manifests/clic...exe_baa8013a79450f71_0001.0003_none_8554920337a51673.manifest
  ...
  ...
  DELETED LOT OF ROWS
  ...
  ...
```

  
    &nbsp;
  
    We have a lot of files of different types from what seems like Windows machine (AppData, Favorites, Downloads, Desktop&#8230;). We can start step 2 that I mentioned before and recursively search for the flag in the strings of the files.
  
  ```default
Megabeets:/tmp/h4ckit/germany# grep -R 'h4ck' CorpUser
Binary file CorpUser/AppData/Roaming/Skype/live#3aames.aldrich/main.db matches
```

  
    This command iterates recursively all the files in the directory and the sub-directories and grep for the string &#8216;h4ck&#8217;. The command returned that there is a database file that is containing part of the flag. Now let&#8217;s <code>strings</code> command on the file:
  
  ```sh
Megabeets:/tmp/h4ckit/germany# strings CorpUser/AppData/Roaming/Skype/live#3aames.aldrich/main.db | grep h4ck1t
live:black.zogzog blackabauh4ck1t{87e2bc9573392d5f4458393375328cf2}h4ck1t{87e2bc9573392d5f4458393375328cf2}8183ce2902ef71ac62ab02a7c8ec762e6b14e318h4ck1t{87e2bc9573392d5f4458393375328cf2}h4ck1t{87e2bc9573392d5f4458393375328cf2}h4ck1t{87e2bc9573392d5f4458393375328cf2}h4ck1t{87e2bc9573392d5f4458393375328cf2}
```

  
    And we got the flag. Easy, right?
  
    <strong>Flag: </strong><em>h4ck1t{87e2bc9573392d5f4458393375328cf2}</em>
</div>



 [1]: http://ctf.com.ua/data/attachments/CorpUser.zip
 [2]: http://www.megabeets.net/h4ck1t-2016-1n51d3r5-j0b-canada/