---
title: '[CSAW 2016] Kill Writeup'
author: Megabeets
type: posts
date: 2016-09-18T22:00:14+00:00

categories:
  - CSAW CTF 2016
  - Writeups
tags:
  - CSAW-2016
  - CTF
  - Forensics
  - Writeups

---
<div class="chal-body">
    <strong>Description:</strong>
  
  <blockquote>
    <p>
      <em>Is kill can fix? Sign the autopsy file?</em><br /> <em> <a class="chal-file" href="https://ctf.csaw.io/stat./a23ef5ecca7f30b77f59f21dba413b07/kill.pcapng" target="_blank">kill.pcapng</a></em>
    </p>
  </blockquote>
</div>

This challenge was the first in the Forensics category and was very very simple. We are given with what seems like a corrupted `pcapng` file, I wasn&#8217;t able to open it in `Wireshark` nor `Tcpdump`. I ran `strings` on it with a hope to find the flag:

```sh
[Megabeets] /tmp/CSAW/kill# strings kill.pcapng | grep -i flag
=flag{roses_r_blue_violets_r_r3d_mayb3_harambae_is_not_kill}
```


And to my great surprise I got it, the flag was written plain-text in the file.

