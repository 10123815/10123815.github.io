---
layout:     post
title:      "水塘抽样"
date:       2016-09-10
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
---

>有N个元素的链表，事先不知道有多长，写一个函数可以高效地从其中取出k个随机数。

__水塘抽样__：水塘抽样是一系列的随机算法，
其目的在于从包含n个项目的集合S中选取k个样本，其中n为一很大或未知的数量，
尤其适用于不能把所有n个项目都存放到主内存的情况。

```bash
從S中抽取首k項放入「水塘」中                       
對於每一個S[j]項（j ≥ k）：                       
   隨機產生一個範圍從0到j的整數r                        
   若 r < k 則把水塘中的第r項換成S[j]項   
```    

下面证明每一项被抽取的概率均为 _k/n_:                           
对于首先放入水塘的k项，其在遍历结束后任然留在水塘中的概率，即对于每个 _j ≥ k_ 的项，都不会替换它，为：   
![](/img/in-post/2016-09-11-rs/small-than-k.png)
对于每个 _j ≥ k_ 的项，其进入水塘，并且在遍历后留在水塘的概率为：
![](/img/in-post/2016-09-11-rs/big-than-k.png)

leetcode上的一道题：__Linked List Random Node__
>Given a singly linked list, 
return a random node's value from the linked list. Each node must have the same probability of being chosen.

```cpp
class Solution {
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head)
        : head_(head) {
        
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        auto node = head_;
        int index = 0;
        int ans = node->val;
        while (node = node->next) {
            int r = randab(0, ++index);
            if (r == 0) {
                ans = node->val;
            }
        }
        return ans;
    }
private:
    int randab(int a, int b) {
        return (rand() % (b-a+1))+ a;
    }
    ListNode* head_;
};
```