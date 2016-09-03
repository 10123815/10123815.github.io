---
layout:     post
title:      "Count Numbers with Unique Digits"
date:       2016-09-03
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10^n.                  
Example:
Given n = 2, return 91. 
(The answer should be the total numbers in the range of 0 ≤ x < 100, 
excluding [11,22,33,44,55,66,77,88,99])


回朔法，解空间树大概是这样：
![](/img/in-post/2016-09-03-cnud-rs.png)
_注意第一层没有0_

```cpp
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        int max = pow(10, n);
        int ans = 1;
        for (int i = 1; i < 10; i++) {
            ans += dfs(i, max, 1 << i);
        }
        return ans;
    }
    int dfs(int cur, int max, short visit) {
        if (cur < max) {
            int ans = 1;
            for (int i = 0; i < 10; i++) {
                short bit = 1 << i;
                if ((bit & visit) == 0) {   // 别忘了括号！
                    short v = visit | bit;
                    int next = cur * 10 + i;
                    ans += dfs(next, max, v);
                }
            }
            return ans;
        }
        return 0;
    }
};
```