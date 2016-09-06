---
layout:     post
title:      "Different Ways to Add Parentheses"
date:       2016-09-06
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given a string of numbers and operators, 
return all possible results from computing all the different possible ways to group numbers and operators.                      
The valid operators are +, - and *.                                
Example:                    
Input: "2*3-4*5"                       
>
```markdown
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
```
Output: [-34, -14, -10, -10, 10]

分治法，以某个operator讲原来的string划分成两个，分别求这两个string的结果，再合并；            
如果某个string中不含operator，说明是一个数字


```cpp
class Solution {
public:
    void result(const vector<int>& lrs, const vector<int>& rrs, char op, vector<int>& ans) {
        int ln = lrs.size();
        int rn = rrs.size();
        for (int i = 0; i < ln; i++) {
            for (int j = 0; j < rn; j++) {
                int lr = lrs[i];
                int rr = rrs[j];
                switch (op) {
                    case '+': ans.push_back(lr + rr);
                    break;
                    case '-': ans.push_back(lr - rr);
                    break;
                    case '*': ans.push_back(lr * rr);
                }
            }
        }
    }
    bool isop(char op) {
        return op == '+' || op == '-' || op == '*';
    }
    void dwc(const string& input, int i, int j, vector<int>& ans) {
        bool hasop = false;
        for (int k = i + 1; k < j; k++) {
            if (isop(input[k])) {
                hasop = true;
                vector<int> lrs;
                dwc(input, i, k, lrs);
                vector<int> rrs;
                dwc(input, k + 1, j, rrs);
                result(lrs, rrs, input[k], ans);
            }
        }
        // If no operators.
        if (!hasop) {
            int res = stoi(input.substr(i, j - i));
            ans.push_back(res);
        }
        
    }
    vector<int> diffWaysToCompute(string input) {
        vector<int> ans;
        dwc(input, 0, input.size(), ans);
        return ans;
    }
};
```