---
layout:     post
title:      "new & delete"
date:       2016-08-06
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - c++
        - 内存管理
---

### new表达式 & delete表达式

+ new表达式执行三步操作
 1. 调用标准库函数`operator new`分配一块大的，原始的，未命名的内存
 2. 运行相应的构造函数，传入初值
 3. 对象被分配了空间并构造完成，返回指向该对象的指针

+ delete表达式执行了两步操作
 1. 执行析构函数
 2. 调用标准库函数`operator delete`释放内存

* _重载`operator new`函数，并将其声明为private，可阻止在堆上创建对象(注意该函数的第一个参数一定是`size_t`，返回值`void*`)_
* _将析构函数声明为private可阻止在栈上创建对象(编译器在为类对象分配栈空间时，会先检查类的析构函数的访问性，其实不光是析构函数，只要是非静态的函数，编译器都会进行检查)_

Clang libcxx的实现

```c++
void * operator new(std::size_t size) throw(std::bad_alloc) {
    if (size == 0)
        size = 1;
    void* p;
    while ((p = ::malloc(size)) == 0) {
        std::new_handler nh = std::get_new_handler();
        if (nh)
            nh();
        else
            throw std::bad_alloc();
    }
    return p;
}

void operator delete(void* ptr) {
    if (ptr)
        ::free(ptr);
}
```

#### 定位new表达式
可以在自定义`operator new`函数时提供额外的形参，此时使用这些自定义函数的new表达式必须使用new的定位形式

```c++
new (place_address) type
new (place_address) type (initializers)
new (place_address) type [size]
new (place_address) type [size] { braced initializers list } 
```

`place_address`必须是一个指针；事实上，定位new允许我们在一个特定的，预先分配的内存地址上构造对象
