---
layout:     post
title:      "Decode String"
date:       2016-09-09
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given an encoded string, return it's decoded string.                 
The encoding rule is: k[encoded\_string],
where the encoded\_string inside the square brackets is being repeated exactly k times. 
Note that k is guaranteed to be a positive integer.                                          
You may assume that the input string is always valid; No extra white spaces, 
square brackets are well-formed, etc.                       
Furthermore, you may assume that the original data does not contain any digits 
and that digits are only for those repeat numbers, k. For example, there won't be input like 
`3a` or `2[4]`.                                
Examples:
>
```cpp
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

每找到一对[],递归

```cpp
class Solution {
public:
    bool isint(char ch) {
        return ch >= '0' && ch <= '9';
    }
    string dfs(int i, int j, const string& s) {
        string ans = "";
        int rep = 0;
        for (int m = i; m < j; m++) {
            if (s[m] == '[') {
                string subans = "";
                int count = 1;
                for (int n = m + 1; n < j; n++) {
                    if (s[n] == '[') count++;
                    else if (s[n] == ']') {
                        if (--count == 0) {
                            if (rep > 0) subans = dfs(m + 1, n, s);
                            m = n;
                            break;
                        }
                    }
                }
                while (--rep >= 0) {
                    ans += subans;
                }
                rep = 0;
            } else if (isint(s[m])) {
                rep = rep * 10 + (s[m] - '0');
            } else {
                ans += s[m];
            }
        }
        return ans;
    }
    string decodeString(string s) {
        return dfs(0, s.size(), s);
    }
};
```