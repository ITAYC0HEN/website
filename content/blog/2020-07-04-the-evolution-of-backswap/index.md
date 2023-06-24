---
title: The Evolution of BackSwap
author: Megabeets
type: posts
date: 2020-07-04T14:14:33+00:00
excerpt: 'The BackSwap banker has been in the spotlight recently due to its unique and innovative techniques to steal money from victims while staying under the radar and remaining undetected. '

featured_image: ./backswap.jpg
categories:
  - Articles
  - Malware
tags:
  - backswap
  - Malware

---
The BackSwap banker has been in the spotlight recently due to its unique and innovative techniques to steal money from victims while staying under the radar and remaining undetected. This malware was previously spotted targeting banks in Poland but has since moved entirely to focus on banks in Spain. The techniques used by it were thoroughly described by our fellow researchers at the [Polish CERT][1] and Michal Poslusny from ESET, who [revealed][2] and coined the malware’s name earlier this year. However after witnessing ongoing  improvements to its malicious techniques we decided to share this information to the wider research community.

In the following research paper, we will focus on the evolution of BackSwap, its uniqueness, successes, and even failures. We will try to give an overview of the malware’s different versions and campaigns, while outlining its techniques, some of which were proven inefficient and dropped soon after their release by the developers. We will also share a detailed table of IOC and a Python3 script used to extract relevant information from BackSwap’s samples.

_This research was done and published by me on Check Point Research Blog_

{{< read_externally url="https://research.checkpoint.com/2018/the-evolution-of-backswap" name="Check Point Research" >}}



 [1]: https://www.cert.pl/en/news/single/backswap-malware-analysis/
 [2]: https://www.welivesecurity.com/2018/05/25/backswap-malware-empty-bank-accounts/