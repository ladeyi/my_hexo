---
title: challenge level 10
date: 2018-6-28 15:00:00
categories: python
tags: code
toc: true
---

## [问题](http://www.pythonchallenge.com/pc/return/bull.html) http://www.pythonchallenge.com/pc/return/bull.html
## challenge 10 解答思路

### Regular Expression Solutions

Here's a short & sweet way to get the whole job done, exploiting that regexps have a natural way to say "find the longest sequence starting at the current position consisting of repetitions of the current digit". The "(\d)" matches a digit as group #1, and the "\1" matches the same thing as group #1. Group #0, or m.group(0), is the entire string.
```python
import re
def describe(s):
    return "".join([str(len(m.group(0))) + m.group(1)
        for m in re.finditer(r"(\d)\1*", s)])
s = "1"
for dummy in range(30):
    s = describe(s)
print len(s)  # prints 5808
```

Here's another version of the regular expression portion, broken into two lines. This shows there are multiple ways to write this, and usually a concise way without explicit for loops.

```python
def describe(s):
    sets = re.findall("(1+|2+|3+)", s)  # like "111", "2", ...
    return "".join([str(len(x))+x[0] for x in sets])
```
<!-- more -->

### List and reduce

```python
def push_one(seq, newch):
    if len(seq)==0 or seq[-1][0]!=newch:
        seq.append((newch,1))
    else:
        seq[-1]=(newch,seq[-1][1]+1)
    return seq

def getnext(x):
    next = reduce(push_one, [[]] + [c for c in x])
    return "".join(map(lambda pair: '%d%c' % (pair[1],pair[0]), next))

l=[]
x='1'
for i in range(40):
    l.append(x)
    x = getnext(x)
print len(l[30]) # prints 5808
```

### Solution using generators
```python
import itertools

def compress(g):
    for key, subgenerator in itertools.groupby(g):
        yield len(list(subgenerator))
        yield key

def mk_recurse_n(f, n):
   def inner(g):
       for i in range(n):
           g = f(g)
       return g
   return inner

g = mk_recurse_n(compress, 30)([1])
a = list(g)
print len(a)
```

### Other solutions

```python
import re
from itertools import groupby
from functools import reduce

a = '1'
b = ''
for i in range(30):
    j = 0
    k = 0
    while j < len(a):
        while k < len(a) and a[k] == a[j]:
            k += 1
        b += str(k-j) + a[j]
        j = k
    a = b
    b = ''
print(len(a))

x = '1'
for k in range(30):
    x = "".join([str(len(i + j)) + i for i, j in re.findall(r"(\d)(\1*)", x)])
print(len(x))


y = '1'
for h in range(30):
    y = "".join([str(len(list(j))) + i for i, j in groupby(y)])
print(len(y))


res = len(reduce(lambda m, n: reduce(lambda u, v: u + str(len(list(v[1]))) + v[0], groupby(x), ""), range(30), "1"))
print(res)
```
