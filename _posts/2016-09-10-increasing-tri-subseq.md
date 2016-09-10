---
layout:     post
title:      "Increasing Triplet Subsequence"
date:       2016-09-06
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given an unsorted array return whether an increasing subsequence of 
length 3 exists or not in the array.

遍历数组，维护一个最小值，和倒数第二小值.                
遍历原数组的时候，如果当前数字小于等于最小值，更新最小值;           
如果严格大于最小值，且小于等于倒数第二小值，更新倒数第二小值;            
如果当前数字比最小值和倒数第二小值都大，
说明此时有三个递增的子序列了，直接返回ture

```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int ai = INT_MAX;
        int aj = INT_MAX;
        for (int i = 0, n = nums.size(); i < n; i++) {
            if (nums[i] <= ai) {
                ai = nums[i];
            } else if (nums[i] <= aj) {
                aj = nums[i];
            } else
                return true;
        }
        return false;
    }
};
```