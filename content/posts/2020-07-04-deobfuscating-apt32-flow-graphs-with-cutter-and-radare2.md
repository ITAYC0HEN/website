---
title: Deobfuscating APT32 Flow Graphs with Cutter and Radare2
author: Megabeets
type: post
date: 2020-07-04T14:25:38+00:00
url: /deobfuscating-apt32-flow-graphs-with-cutter-and-radare2/
featured_image: /uploads/apt32deobf.jpg
categories:
  - Articles
  - Malware
  - radare2
  - Tools

---
The Ocean Lotus group, also known as APT32, is a threat actor which has been known to target East Asian countries such as Vietnam, Laos and the Philippines. The group strongly focuses on Vietnam, especially private sector companies that are investing in a wide variety of industrial sectors in the country. While private sector companies are the group’s main targets, APT32 has also been known to target foreign governments, dissidents, activists, and journalists.

APT32’s toolset is wide and varied. It contains both advanced and simple components; it is a mixture of handcrafted tools and commercial or open-source ones, such as Mimikatz and Cobalt Strike. It runs the gamut from droppers, shellcode snippets, through decoy documents and backdoors. Many of these tools are highly obfuscated and seasoned, augmented with different techniques to make them harder to reverse-engineer.

In this article, we get up and close with one of these obfuscation techniques. This specific technique was used in a backdoor of Ocean Lotus’ tool collection. We’ll describe the technique and the difficulty it presents to analysts — and then show how bypassing this kind of technique is a matter of writing a simple script, as long as you know what you are doing.





<div class="wp-block-uagb-marketing-button uagb-marketing-btn__outer-wrap uagb-marketing-btn__align-center uagb-marketing-btn__align-text-center uagb-marketing-btn__icon-before uagb-block-beea85a6">
  <div class="uagb-marketing-btn__wrap">
    <a href="https://research.checkpoint.com/2019/deobfuscating-apt32-flow-graphs-with-cutter-and-radare2/" class="uagb-marketing-btn__link" target="_blank" rel="noopener noreferrer">
    
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



<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img class="wp-image-149 alignnone" src="https://www.megabeets.net/uploads/megabeets_inline_logo.png" alt="megabeets_inline_logo" width="61" height="45" />Eat Veggies</a>
  </p>
</div>