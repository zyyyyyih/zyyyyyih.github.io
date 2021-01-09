---
title: C++的STL
tags:
  - C++
  - STL
categories:
  - 学习
abbrlink: 553b08ef
date: 2021-01-09 13:54:54
---
# 不定长数组：vector
vector就是一个不定长数组，也就是动态数组，不仅如此，它把一些常用操作“封装”在了vector类型内部。
定义：`vector<变量类型> 变量名`
例如：现在定义一个`vector<int> a`则
`a.size()`可以读取它的大小
`a.resize()`改变大小
`a.push_back()`向尾部添加元素
`a.pop_back()`删除最后一个元素