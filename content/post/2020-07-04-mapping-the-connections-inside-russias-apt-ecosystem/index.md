---
title: Mapping the connections inside Russia’s APT Ecosystem
author: Megabeets
type: post
date: 2020-07-04T15:27:48+00:00
url: /mapping-the-connections-inside-russias-apt-ecosystem/
featured_image: /uploads/russian_apt_ecosystem_map.png
categories:
  - Articles
  - Malware

---
If the names Turla, Sofacy, and APT29 strike fear into your heart, you are not alone. These are known to be some of the most advanced, sophisticated and notorious APT groups out there – and not in vain. These Russian-attributed actors are part of a bigger picture in which Russia is one of the strongest powers in the cyber warfare today. Their advanced tools, unique approaches, and solid infrastructures suggest enormous and complicated operations that involve different military and government entities inside Russia.&nbsp;

Russia is known to conduct a wide range of cyber espionage and sabotage operations for the last three decades. Beginning with the first publicly known attacks by [Moonlight Maze][1], in 1996, going through the [Pentagon breach][2] in 2008, [Blacking out Kyiv][3] in 2016, Hacking the [US Elections][4] in 2016, and up to some of the largest most infamous cyberattacks in history – [targeting a whole country][5] with NotPetya ransomware.

Indeed, numerous Russian operations and malware families were publicly exposed by different security vendors and intelligence organizations such as the [FBI][6] and the [Estonian Foreign Intelligence Services][7]. While all of these shed light on specific Russian actors or operations, the bigger picture remains hazy.

The fog behind these complicated operations made us realize that while we know a lot about single actors, we are short of seeing a whole ecosystem with actor interaction (or lack thereof) and particular TTPs that can be viewed in a larger scope. We decided to know more and to look at things from a broader perspective. This led us to gather, classify and analyze thousands of Russian APT malware samples in order to find connections – not only between samples but also between different families and actors.&nbsp;

During this research, we analyzed approximately 2,000 samples that were attributed to Russia and found 22,000 connections between the samples and 3.85 million non-unique pieces of code that were shared. We classified these samples into 60 families and 200 different modules.

<div style="height:23px" aria-hidden="true" class="wp-block-spacer">
</div>



<div class="wp-block-uagb-marketing-button uagb-marketing-btn__outer-wrap uagb-marketing-btn__align-center uagb-marketing-btn__align-text-center uagb-marketing-btn__icon-before uagb-block-01715af8">
  <div class="uagb-marketing-btn__wrap">
    <a href="https://research.checkpoint.com/2019/russianaptecosystem/" class="uagb-marketing-btn__link" target="_blank" rel="noopener noreferrer">
    
    <div class="uagb-marketing-btn__title-wrap">
      <div class="uagb-marketing-btn__icon-wrap">
        <svg xmlns="http://www.w3.org/2000/svg" viewbox="0 0 512 512"><path d="M432,320H400a16,16,0,0,0-16,16V448H64V128H208a16,16,0,0,0,16-16V80a16,16,0,0,0-16-16H48A48,48,0,0,0,0,112V464a48,48,0,0,0,48,48H400a48,48,0,0,0,48-48V336A16,16,0,0,0,432,320ZM488,0h-128c-21.37,0-32.05,25.91-17,41l35.73,35.73L135,320.37a24,24,0,0,0,0,34L157.67,377a24,24,0,0,0,34,0L435.28,133.32,471,169c15,15,41,4.5,41-17V24A24,24,0,0,0,488,0Z"></path></svg>
      </div>
      
      <h6 class="uagb-marketing-btn__title">
        Read on Check Point Research
      </h6>
    </div>
    
    <div class="uagb-marketing-btn__prefix-wrap">
      <p class="uagb-marketing-btn__prefix">
        <em><em>This research was done and published by me on Check Point Research Blog</em></em>
      </p>
    </div></a>
  </div>
</div>

<div style="height:67px" aria-hidden="true" class="wp-block-spacer">
</div>

<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>

 [1]: https://media.kasperskycontenthub.com/wp-content/uploads/sites/43/2018/03/07180251/Penquins_Moonlit_Maze_PDF_eng.pdf
 [2]: https://web.archive.org/web/20090221124901/https://articles.latimes.com/2008/nov/28/nation/na-cyberattack28
 [3]: https://www.wired.com/story/crash-override-malware/
 [4]: https://www.dhs.gov/news/2016/10/07/joint-statement-department-homeland-security-and-office-director-national
 [5]: https://www.washingtonpost.com/world/national-security/russian-military-was-behind-notpetya-cyberattack-in-ukraine-cia-concludes/2018/01/12/048d8506-f7ca-11e7-b34a-b85626af34ef_story.html?
 [6]: https://www.us-cert.gov/sites/default/files/publications/JAR_16-20296A_GRIZZLY%20STEPPE-2016-1229.pdf
 [7]: https://www.valisluureamet.ee/pdf/raport-2018-ENG-web.pdf