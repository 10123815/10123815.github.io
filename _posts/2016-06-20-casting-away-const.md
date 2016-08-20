---
layout:     post
title:      "Casting Away Constness"
date:       2016-06-20
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - c++
---

#### 常量性转除

避免代码重复的安全做法：令non-const成员调用const成员

```c++
class TextBlock {
    public:
    ...
    const char& operator[] (std::size_t position) const
    {
        ...
        ...
        return text[position];
    }
    
    char& operator[] (std:;size_t position)
    {
        return 
        const_cast<char&>(                          // 将const版本[]返回的对象的const移除
            static_cast<const TextBlock&>(*this)    // 将传入的对象转成const，避免其递归调用non-const版本的[]
            [position]
        );
    }
};
```

令const版本调用non-const版本是错误的：const承诺不改变对象的逻辑状态，non-const却不会
