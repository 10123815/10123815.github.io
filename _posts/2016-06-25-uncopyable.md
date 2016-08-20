---
layout:     post
title:      "Uncopyable base class"
date:       2016-06-25
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - c++
---

将copy构造函数声明为`private`并不绝对安全，因为成员函数和友元函数还是可以调用；如果不实现它们，对其调用将获得一个链接错误。

一个阻止copying动作设计的base class

```c++
class Uncopyable 
{
protected:
   Uncopyable() { }
   ~Uncopyable() { }
private:
    Uncopyable(const Uncopyable&);
    Uncopyable& operator=(const Uncopyable&);
}
```

为阻止某个对象被拷贝，令其继承Uncopyable.<br/>
这样将链接错误移至编译期。
