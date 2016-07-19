---
layout:     post
title:      "Container With Most Water"
date:       2016-06-18
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - two pointer
---
>给出一个非零数组，代表一系列高度；
取出两个高度，计算两个高度和低组成的容器最多能装多少水。

*Note*：两个高度中较低的那个最终决定水槽的容量！

设i，j分别为水槽的左右两个高度，i，j中较小的向中间移动，
直到找到新的i或j比之前较大的还要大，此时底比之前要小，
最小高度比之前高，容量可能比之前大。

```python
def computeArea(self, l, r, x):
    return l * x if l <= r else r * x
def maxArea(self, h):
    L = len(h)
    i, j = 0, L - 1
    ma = self.computeArea(h[i], h[j], L - 1)
    while i < j:
        if h[i] <= h[j]:
            i += 1
        else:
            j -= 1
        a = self.computeArea(h[i], h[j], j - i)
        if a > ma:
            ma = a
    return ma
```