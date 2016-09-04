---
layout:     post
title:      "Combination Sum"
date:       2016-09-03
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given a set of candidate numbers (C) and a target number (T), 
find all unique combinations in C where the candidate numbers sums to T.     
The same repeated number may be chosen from C unlimited number of times.     
For example, given candidate set [2, 3, 6, 7] and target 7, a solution set is:      
[
  [7],
  [2, 2, 3]
]

回朔法，先对数组排序，搜索时只找比自己大的，避免结果中有重复

```cpp
class Solution {
public:
    typedef vector<vector<int>> Ans;
    void search(vector<int>& c, int t, int start, vector<int>& res, Ans& ans) {
        for (size_t i = start; i < n_; i++) {
            if (c[i] < t) {
                vector<int> newres;
                newres.assign(res.begin(), res.end());
                newres.push_back(c[i]);
                search(c, t - c[i], i, newres, ans);
            } else if (c[i] == t) {
                res.push_back(t);
                ans.push_back(res);
            }
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        n_ = candidates.size();
        sort(candidates.begin(), candidates.end());
        Ans ans;
        for (size_t i = 0; i < n_; i++) {
            if (candidates[i] > target) {
                break;
            } else if (candidates[i] == target) {
                ans.push_back({target});
                break;
            }
            vector<int> res = {candidates[i]};
            search(candidates, target - candidates[i], i, res, ans);
        }
        return ans;
    }
    
private:
    size_t n_;
};
```