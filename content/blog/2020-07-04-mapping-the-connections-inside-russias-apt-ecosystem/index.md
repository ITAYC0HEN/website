---
title: Mapping the connections inside Russia’s APT Ecosystem
author: Megabeets
type: posts
date: 2020-07-04T15:27:48+00:00

featured_image: ./russian_apt_ecosystem_map.png
categories:
  - Articles
  - Malware
---
If the names Turla, Sofacy, and APT29 strike fear into your heart, you are not alone. These are known to be some of the most advanced, sophisticated and notorious APT groups out there – and not in vain. These Russian-attributed actors are part of a bigger picture in which Russia is one of the strongest powers in the cyber warfare today. Their advanced tools, unique approaches, and solid infrastructures suggest enormous and complicated operations that involve different military and government entities inside Russia.&nbsp;

Russia is known to conduct a wide range of cyber espionage and sabotage operations for the last three decades. Beginning with the first publicly known attacks by [Moonlight Maze][1], in 1996, going through the [Pentagon breach][2] in 2008, [Blacking out Kyiv][3] in 2016, Hacking the [US Elections][4] in 2016, and up to some of the largest most infamous cyberattacks in history – [targeting a whole country][5] with NotPetya ransomware.

Indeed, numerous Russian operations and malware families were publicly exposed by different security vendors and intelligence organizations such as the [FBI][6] and the [Estonian Foreign Intelligence Services][7]. While all of these shed light on specific Russian actors or operations, the bigger picture remains hazy.

The fog behind these complicated operations made us realize that while we know a lot about single actors, we are short of seeing a whole ecosystem with actor interaction (or lack thereof) and particular TTPs that can be viewed in a larger scope. We decided to know more and to look at things from a broader perspective. This led us to gather, classify and analyze thousands of Russian APT malware samples in order to find connections – not only between samples but also between different families and actors.&nbsp;

During this research, we analyzed approximately 2,000 samples that were attributed to Russia and found 22,000 connections between the samples and 3.85 million non-unique pieces of code that were shared. We classified these samples into 60 families and 200 different modules.

{{< read_externally url="https://research.checkpoint.com/2019/russianaptecosystem/" name="Check Point Research" >}}




 [1]: https://media.kasperskycontenthub.com/wp-conte./sites/43/2018/03/07180251/Penquins_Moonlit_Maze_PDF_eng.pdf
 [2]: https://web.archive.org/web/20090221124901/https://articles.latimes.com/2008/nov/28/nation/na-cyberattack28
 [3]: https://www.wired.com/story/crash-override-malware/
 [4]: https://www.dhs.gov/news/2016/10/07/joint-statement-department-homeland-security-and-office-director-national
 [5]: https://www.washingtonpost.com/world/national-security/russian-military-was-behind-notpetya-cyberattack-in-ukraine-cia-concludes/2018/01/12/048d8506-f7ca-11e7-b34a-b85626af34ef_story.html?
 [6]: https://www.us-cert.gov/sites/default/files/publications/JAR_16-20296A_GRIZZLY%20STEPPE-2016-1229.pdf
 [7]: https://www.valisluureamet.ee/pdf/raport-2018-ENG-web.pdf