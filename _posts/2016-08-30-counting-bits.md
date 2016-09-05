---
layout:     post
title:      "Counting Bits"
date:       2016-08-30
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given a non negative integer number num.                                   
For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.                          
Example:                     
For num = 5 you should return [0,1,1,2,1,2].                   
Follow up:
>
+ It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
+ Space complexity should be O(n).
+ Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.

利用[或与加](https://10123815.github.io/2016/08/28/or&and/)中的
_a + b = a \| b_             
对每一个数 _n_，等于比其小的最大的2的指数 _a_ 与 _n - a_ 的和，此时 _n = a \| (n - a) = a + (n - a)_            
所以有，_ans[n] = ans[a] + ans[n - a]_

```cpp
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> ans(num + 1);
        if (num == 0) {
            return ans;
        }
        ans[1] = 1;
        int bit = 2;    // 记录比i大的最小的2的指数
        for (size_t i = 2; i <= num; i++) {
            if (bit & i) {
                bit <<= 1;
                ans[i] = 1;
                continue;
            }
            int a = bit >> 1;   // a就是比i小的最大的2的指数
            int b = i - a;
            ans[i] = ans[a] + ans[b];
        }
        return ans;
    }
};
```