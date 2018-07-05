---
title: challenge level 8
date: 2018-7-5 16:00:00
categories: python
tags: code
toc: true
---

## [问题](http://www.pythonchallenge.com/pc/def/integrity.html) http://www.pythonchallenge.com/pc/def/integrity.html
## challenge 8 解答思路

### Use bz2

```python
import bz2
name = bz2.decompress(b'BZh91AY&SYA\xaf\x82\r\x00\x00\x01\x01\x80\x02\xc0\x02\x00 \x00!\x9ah3M\x07<]\xc9\x14\xe1BA\x06\xbe\x084')
psd = bz2.decompress(b'BZh91AY&SY\x94$|\x0e\x00\x00\x00\x81\x00\x03$ \x00!\x9ah3M\x13<]\xc9\x14\xe1BBP\x91\xf08')
print(name)
print(psd)
```

<!-- more -->

### BASH Shell Script
```bash
#!/bin/bash
un="$(curl -s http://www.pythonchallenge.com/pc/def/integrity.html%7Csed -nE "/^un.*$/ s/^.*'(.*)'.*$/\1/p")";
pw="$(curl -s http://www.pythonchallenge.com/pc/def/integrity.html%7Csed -nE "/^pw.*$/ s/^.*'(.*)'.*$/\1/p")";
t=`mktemp`;
for i in "$un" "$pw"; do
        printf "$i">$t;
        bzcat $t;
        echo;
done;
```
```bash
#!/bin/bash
for s in un: pw:; do
  printf $s
  printf "`curl -s http://www.pythonchallenge.com/pc/def/integrity.html | grep $s | cut -d\' -f2`" | bzcat
  echo
done
```