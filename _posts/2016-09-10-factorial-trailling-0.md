---
layout:     post
title:      "Factorial Trailing Zeroes"
date:       2016-09-06
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given an integer n, return the number of trailing zeroes in n!.

返回n的阶乘后面有几个0。             
只有 _2 * 5_ 才能得到 _0_，这道题实际上是看n!里有几个 _2 * 5_ ，比如 _5! = 2 * 2 * 2 * 3 * 5_ ，有一对 _2 * 5_ ,由于2远比5多，只要看5的个数。                              
这道题的难点在于5的指数会包含多个5，比如25提供了2个5，125提供了3个5，所以最终要算的是:    
只提供一个5的数的个数 * 1，加上提供2个5的数的个数 * 2....

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        unsigned long long nn = 1;
        int pow = 0;
        while (nn <= n) {
            nn *= 5;
            pow++;
        }
        nn /= 5;    // 最大的5的指数
        pow--;
        int ans = 0;
        int lastcount = 0;
        while (pow > 0) {
            int count = n / nn;                 
            ans += (count - lastcount) * pow--;
            nn /= 5;
            lastcount = count;
        }
        return ans;
    }
};
```