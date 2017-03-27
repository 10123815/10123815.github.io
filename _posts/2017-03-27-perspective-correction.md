---
layout:     post
title:      "透视矫正"
date:       2017-03-27
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:
        - 图形学
---

![](/img/in-post/2017-03-27-perspective-correction/pc.jpg)
投影面上纹理坐标u、v与顶点点坐标x、y __不是__ 线性关系<br/>

实际上，s/z、t/z和x’、y’也是线性关系：对1/z关于x’、y’插值得到1/z’，然后对s/z、t/z关于x’、y’进行插值得到s’/z’、t’/z’，然后用s’/z’和t’/z’分别除以1/z’，就得到了插值s’和t’。（带’的是投影空间种的坐标，不带的是相机空间的坐标）<br/>

除了纹理坐标，顶点的其他属性也是同样的道理