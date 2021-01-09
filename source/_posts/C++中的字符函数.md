---
title: C++中的字符函数
tags: C++
categories: 学习
abbrlink: 417c2b55
date: 2020-08-17 19:53:19
---
## 长度计算
### `strlen(s1)`
作用：得到s1的实际长度
```cpp
    char s1[20];
    cin >> s;//eg:hello
    int len = strlen(s1);
    cout << len << endl;//结果为 5
```
## 复制
### `strcpy(s1, s2);`
作用：将s2中的内容完全放入s1中，s1原来的内容不会被保留，可以理解为赋值符号
- 没有要求s2一定要比s1短或者长

```cpp
    char s1[] = "Hello world";
    char s2[] = "hello";
    strcpy(s1, s2);
    cout << s1 << endl << s2 << endl;//结果输出两行hello
```
### `strncpy(s1, s2, n);`
作用：将s2的前n个字符替换进s1的前n个中
- 需要满足n小于等于s1，s2的长度才可以正常使用

正常：
```cpp
    char s1[] = "hello world";
    char s2[] = "I'm LiHua";
    strncpy(s1,s2,3);
    cout<< s1<<endl;//输出I'mlo world
```
若n超出s2长度
```cpp
    char s1[] = "hello world";//长度11
    char s2[] = "I'm LiHua";//长度9
    strncpy(s1,s2,11);//若大于12也为此结果
    cout<< s1<<endl;//输出I'm LiHua
```
若s1长度小于s2，n又超过了s1长度则不合法，无法得到正常结果
但有一种情况是正常的
```cpp
    char s1[17] = "hello world";//给定长度
    char s2[] = "I'm LiHuaaaaaaaaaaa";
    strncpy(s1,s2,14);//未超过给定长度17，但是超过了字符串实际长度11
    cout<< s1<<endl;
```
## 连接
### `strcat(s1, s2);`


## 比较`strcmp(s1, s2);`
## 查找`strchr(s1, ch);`
## 字串`strstr(s1, s2);`
