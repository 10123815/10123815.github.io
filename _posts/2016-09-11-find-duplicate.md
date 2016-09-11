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

>Given an array nums containing _n + 1_ integers 
where each integer is between 1 and n (inclusive), 
prove that at least one duplicate number must exist. 
Assume that there is only one duplicate number, 
find the duplicate one.

找到唯一的一个重复的数字。            
因为最大的数为 _n_, 而下标的范围为 _0 ~ n_,
所以可以用某个数字作为一个下标，指向下一个数。             
而重复的数字将指向同一个数，不重复的数字一定不会指向同一个数。            
比如数组 `[2,4,3,1,2,5]`，两个2都指向3，
如果从第一个数字2开始便利数组，就会得到一个链表：            
`2->3->1->4->2->3->...`，即这个链表出现环了，可以用快慢指针解决。

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0;
        int fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        int i = 0;
        while (i != slow) {
            i = nums[i];
            slow = nums[slow];
        }
        return i;
    }
};
```