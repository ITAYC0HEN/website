---
title: Deobfuscating APT32 Flow Graphs with Cutter and Radare2
author: Megabeets
type: posts
date: 2020-07-04T14:25:38+00:00

featured_image: ./apt32deobf.jpg
categories:
  - Articles
  - Malware
  - radare2
  - Tools

---
The Ocean Lotus group, also known as APT32, is a threat actor which has been known to target East Asian countries such as Vietnam, Laos and the Philippines. The group strongly focuses on Vietnam, especially private sector companies that are investing in a wide variety of industrial sectors in the country. While private sector companies are the group’s main targets, APT32 has also been known to target foreign governments, dissidents, activists, and journalists.

APT32’s toolset is wide and varied. It contains both advanced and simple components; it is a mixture of handcrafted tools and commercial or open-source ones, such as Mimikatz and Cobalt Strike. It runs the gamut from droppers, shellcode snippets, through decoy documents and backdoors. Many of these tools are highly obfuscated and seasoned, augmented with different techniques to make them harder to reverse-engineer.

In this article, we get up and close with one of these obfuscation techniques. This specific technique was used in a backdoor of Ocean Lotus’ tool collection. We’ll describe the technique and the difficulty it presents to analysts — and then show how bypassing this kind of technique is a matter of writing a simple script, as long as you know what you are doing.


{{< read_externally url="https://research.checkpoint.com/2019/deobfuscating-apt32-flow-graphs-with-cutter-and-radare2/" name="Check Point Research" >}}
