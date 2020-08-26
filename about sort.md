``` Python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

def timer(func):
    import functools
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        import time
        t1 = time.time()
        func(*args, **kwargs)
        t2 = time.time()
        print(f"{func.__name__}: {t2 - t1} secs")
    return wrapper

@timer
def bubble(a):
    for i in range(len(a)):
        for j in range(len(a) - i - 1):
            if a[j] > a[j + 1]:
                a[j], a[j + 1] = a[j + 1], a[j]

@timer
def normal(a):
    for i in range(len(a)):
        for j in range(i, len(a)):
            if a[i] > a[j]:
                a[i], a[j] = a[j], a[i]

@timer
def qsort(a):
    l = 0
    r = len(a) - 1
    def _qsort(a, l, r):
        if not a or l >= r: return
        m = a[l]; i = l; j = r
        while i < j:
            while i < j and a[j] > m: j -= 1
            if i < j: a[i] = a[j]; i += 1
            while i < j and a[i] < m: i += 1
            if i < j: a[j] = a[i]; j -= 1
        a[i] = m
        _qsort(a, l, i - 1)
        _qsort(a, i + 1, r)
    _qsort(a, l, r)

import sys
sys.setrecursionlimit(15000)

import random
a = [random.randint(0, 100000) for x in range(2000)]

normal(a[:])
bubble(a[:])
qsort(a[:])
```

result:
```
running [py.py] ...

normal: 1.0199034214019775 secs
bubble: 1.4529473781585693 secs
qsort: 0.012239456176757812 secs

Press ENTER or type command to continue
```

why bubble is so slow? what's the name of *normal* sort?
