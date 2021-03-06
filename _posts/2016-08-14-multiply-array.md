---
layout:     post
title:      "构建乘积数组"
date:       2016-08-14
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
---

>给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],
其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法；不能使用额外的空间

对于每个B的元素B[i]，使用两次循环计算，第一次计算A[i]左面全部数的乘积，第二次计算右面的乘积

```c++
vector<int> multiply(const vector<int>& A) {
    size_t l = A.size();
    vector<int> B(l);
    B[0] = 1;
    for (int i = 1; i < l; ++i) {
        B[i] = B[i - 1] * A[i - 1];
    }
    for (int i = l - 2; i > 0; --i) {
        B[0] *= A[i + 1];
        B[i] *= B[0];
    }
    B[0] *= A[1];
    return B;
}
```