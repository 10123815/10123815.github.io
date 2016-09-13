---
layout:     post
title:      "Burst Ballons"
date:       2016-09-11
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given n balloons, indexed from 0 to n-1. 
Each balloon is painted with a number on it represented by array nums. 
You are asked to burst all the balloons. 
If the you burst balloon i you will get `nums[left] * nums[i] * nums[right]` coins. 
Here `left` and `right` are adjacent indices of i. 
After the burst, the `left` and `right` then becomes adjacent.  
>                
>
__Note:__
    1. You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.
    2. `0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100`                                       
>__Example:__
>
```markdown
 nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

手贱点开了一道hard的题...

```cpp
class Solution {
public:
    typedef vector<vector<int>> DP;
    int gao(vector<int>& nums, int beg, int end, DP& dp) {
        if (dp[beg][end] > 0) return dp[beg][end];
        for (int i = beg; i <= end; i++) {
            dp[beg][end] = max(dp[beg][end], 
            gao(nums, beg, i - 1, dp) + nums[beg - 1] * nums[i] * nums[end + 1] + gao(nums, i + 1, end, dp));
        }
        return dp[beg][end];
    }
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.begin(), 1);
        nums.insert(nums.end(), 1);
        DP dp(n + 2, vector<int>(n + 2, 0));
        return gao(nums, 1, n, dp);
    }
};
```