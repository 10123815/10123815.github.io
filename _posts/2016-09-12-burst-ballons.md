---
layout:     post
title:      "Burst Ballons"
date:       2016-09-06
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