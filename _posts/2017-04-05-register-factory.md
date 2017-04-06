---
layout:     post
title:      "c++自动注册工厂模式"
date:       2017-04-05
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:
        - c++
---

```cpp

// item_factory.h
class Item
{
        // Item基类
};

class ItemFactory
{
public:
        template<typename T>
        struct Register
        {
                Register(unsigned int id) {
                        ItemFactory::Instance().item_creators_[id] = []{
                                // 只能注册无参的构造函数
                                return new T;
                        }
                }
        }

        // 单例

        // Create函数

private:
        // id到构造函数的map
};

// 创建一个Register对象，类型是ItemFacory::Register<CLASS>，对象名_item_##CLASS##_，参数是KEY
// 在Register的构造函数内部，实现了类型CLASS的构造函数的注册
// ##连接左右形成一个
// 如果想使用带参数的构造函数，使用__VA_ARGS__
#define REGISER_ITEM_CREATOR(CLASS, KEY) static ItemFacory::Register<CLASS> _item_##CLASS##_(KEY)

// concrete_item.h

class HPPotion
{
        // some function
};

// 注册HPpotion的无参构造函数
REGISER_ITEM_CREATOR(HPPotion, 145);

```