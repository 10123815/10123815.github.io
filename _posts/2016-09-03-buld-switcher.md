---
layout:     post
title:      "Bulb Switcher"
date:       2016-09-03
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>There are n bulbs that are initially off. You first turn on all the bulbs. 
Then, you turn off every second bulb. 
On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). 
For the ith round, you toggle every i bulb. For the nth round, you only toggle the last bulb. 
Find how many bulbs are on after n rounds.

>这道题给了我们n个灯泡，第一次打开所有的灯泡，第二次每两个更改灯泡的状态，第三次每三个更改灯泡的状态，
以此类推，第n次每n个更改灯泡的状态。让我们求n次后，所有亮的灯泡的个数。


对于第i个灯泡，将在其因子次改变状态   
比如第9个灯泡，有因子1,3,9，则在第1次打开，在第3次关掉，第9次在打开，   
因此，如果第i个灯泡，如果i有技术个因子，则最后将处于打开状态，    
而i的因子都是成对出现的，_除了平方数_，因此只需求小于等于n的平方数有几个

```cpp
class Solution {
public:
    int bulbSwitch(int n) {
        return sqrt(n);
    }
};
```