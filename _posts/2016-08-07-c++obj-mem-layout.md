---
layout:     post
title:      "c++对象内存布局"
date:       2016-08-07
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - c++
---

#### 影响对象大小的因素

0. 成员变量     
1. 虚函数表指针`_vftptr`   
2. 虚基类表指针`_vbptr`   
3. 内存对齐

+ `_vftptr`、`_vbptr`的初始化由对象的构造函数, 赋值运算符自动完成；
对象生命周期结束后，由对象的析构函数来销毁。

![](/img/in-post/2016-08-07-coml/uml.png)

```c++
// sizeof(CGrandChildren) = 36
```

![](/img/in-post/2016-08-07-coml/layout.png)