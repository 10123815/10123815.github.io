---
layout:     post
comments:   true
title:      "c++自动注册工厂模式"
date:       2017-04-28
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:
        - c++
---

<pre>
<code>

/// factory.h ///

template<typename ItemType>
class Factory
{
public:
        template<typename T>
        struct Creator
        {
                Creator(const std::string& id) {
                        // 内部类可以通过外部类的实例访问外部类的私有成员
                        Factory::Instance().m_items[id] = new T();
                }
        };

        // template<typename T>
        // struct Register
        // {
        //         Register(cosnt std::string& id) {
        //                 Factory::Instance().m_creators[id] = [] {
        //                         return new T();
        //                 }
        //         }
        // };

        // 单例

        // Create函数，根据id直接返回

private:
        // id到对象实例的map，这里用类名做id
        // 直接保存对象，适用于类成员保持不变，在内存中只需保存一份实例的情况
        std::map<std::string, ItemType*> m_items;

        // 保存类的构造函数，适用于某一个类需要多次创建对象，在内存中有多份实例的情况
        // std::map<std::string, std::function<ItemType*()>> m_creators;
};

/// item.h ///

#include "factory.h"

class Item
{

};

// 创建一个Creator对象，类型是ItemFacory::Creator<CLASS>，对象名_##CLASS##_，参数是KEY
// 类型实例化过程中，在Creator的构造函数内部，创建了类型CLASS的实例
// ##连接左右形成一个字符串
// 如果想使用带参数的构造函数，使用__VA_ARGS__
#define REGISER_ITME(CLASS, KEY) Factory::Creator<CLASS> _##CLASS##_(KEY)


/// concrete_item.h ///

#include "item.h"

class HPPotion : public Item
{
        // some function
};

/// concrete_item.cpp ///

// HPPotion成员定义

// 注册HPpotion的无参构造函数
// 注意这个调用要写在.cpp里。
// 如果写在.h里可能会重复调用，重复创建同样id的对象
// 以至于替换std::map中已存在的同样id的对象，而原来的对象将没有被delete而导致内存泄露
REGISER_ITME(HPPotion, 145);
