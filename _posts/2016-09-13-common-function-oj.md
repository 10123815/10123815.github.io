---
layout:     post
title:      "刷题常用"
date:       2016-09-13
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
---

+ 随机数
+ 
```cpp
#include <stdlib.h>
// 要取得[a,b)的随机整
(rand() % (b-a)) + a; 
// 要取得[a,b]的随机整
(rand() % (b-a+1)) + a; 
// 要取得(a,b]的随机整
(rand() % (b-a)) + a + 1; 
```

+ string int 互转
+            
```cpp
#include <sstream>
stringstream ss;
int n;
string str;
// string to int
str = "asdfasdfasdf";
ss << str;
ss >> n;
// int to string
n = 12312;
ss << n;
ss >> str;
// sprintf
#include <stdio.h>
char buf[10];
sprintf(buf, "%d", n);
// sscanf
char str[] = "15.455";
int i;
float fp;
sscanf(str, "%d", &i);         // i = 15
sscanf(str, "%f", &fp);      // fp = 15.455000
```