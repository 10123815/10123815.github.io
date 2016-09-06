---
layout:     post
title:      "Generate Parentheses"
date:       2016-09-06
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
For example, given n = 3, a solution set is:   
>
```json           
[           
  "((()))",          
  "(()())",            
  "(())()",           
  "()(())",            
  "()()()"            
]
```

只要确定了 _(_ 的位置， _)_ 也确定了。                     
可以用回朔法，状态为 _(_ 在结果中的位置，例如 _n = 4_ 时，某一个状态为013，意味着目前已经确定的某个结果为 _((#(####_          
解空间：
![](/img/in-post/2016-09-06-gp.png)

```cpp
class Solution {
public:
    typedef vector<string> Ans;
    vector<string> generateParenthesis(int n) {
        if (n == 0) return {""};
        if (n == 1) return {"()"};
        Ans ans;
        string res(n * 2, ' ');
        res[0] = '(';
        res[n * 2 - 1] = ')';
        search(1, n, 1, res, ans);
        return ans;
    }
    void search(int count, int n, int cur, string& res, Ans& ans) {
        if (++count == n) {
            for (int i = cur; i < 2 * n - 1; i++) {
                string newres(res);
                newres[i] = '(';
                for (int j = 1; j < 2 * n - 1; j++) {
                    if (newres[j] == ' ') {
                        newres[j] = ')';
                    }
                }
                ans.push_back(newres);
            }
        } else {
            for (int i = cur; i < 2 * count - 1; i++) {
                string newres(res);
                newres[i] = '(';
                search(count, n, i + 1, newres, ans);
            }
        }
    }
};
```