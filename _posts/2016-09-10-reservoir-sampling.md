---
layout:     post
title:      "水塘抽样"
date:       2016-09-06
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

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

$$ \prod _{ j=k }^{ n-1 }{ 1-\frac { 1 }{ j+1 }  } =\frac { k }{ n }  $$            