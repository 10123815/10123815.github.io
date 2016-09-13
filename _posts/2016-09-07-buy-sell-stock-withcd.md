---
layout:     post
title:      "Best Time to Buy and Sell Stock with Cooldown"
date:       2016-09-07
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Say you have an array for which the ith element is the price of a given stock on day i.                             
Design an algorithm to find the maximum profit. You may complete as many transactions as you like 
(ie, buy one and sell one share of the stock multiple times) with the following restrictions:
>
+ You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
+ After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

买股票系列，可以买卖多次，但卖完后要隔一天才能再买

buy[i]表示在到第i天最后一个操作是买，此时的最大收益。                
sell[i]表示在到第i天最后一个操作是卖，此时的最大收益。

```cpp
sells[i] = max(sells[i - 1], buys[i - 1] + prices[i])
buys[i] = max(buys[i - 1], sells[i - 2] - prices[i])
```

由于之和前两个状态有关，不需要两个数组

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n <= 1) return 0;
        
        int lastsell = 0;
        int sell = max(0, prices[1] - prices[0]);
        int lastbuy = -prices[0];
        int buy = max(-prices[0], -prices[1]);
        for (int i = 2; i < n; i++) {
            int newbuy = max(lastsell - prices[i], buy);
            int newsell = max(sell, buy + prices[i]);
            lastbuy = buy;
            buy = newbuy;
            lastsell = sell;
            sell = newsell;
        }
        return sell;
    }
};
```