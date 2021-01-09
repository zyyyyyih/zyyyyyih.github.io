---
title: CCT速成
tags: C++
categories: 学习
abbrlink: ba2a05f9
date: 2020-09-08 13:56:50
---
本文为对CCT同学的个人指导（入门速成）
# 大纲
**C++基本语法（可能会让你读代码说作用）**
- cin，cout
- scanf，printf
- 数组
- char，string
- if else，switch
- bool
- **for**，**while**，*do...while
- 函数
- *指针， *引用
- **struct**， *class
- 深入STL（vector，set，map，queue）

**算法基本概念的了解**
- 图论（图，树）
- *数论（了解线性筛，欧拉筛，欧几里得和扩展欧几里得就够了）
- **搜索（DFS，BFS）**

# 详解C++基本语法
## cin，cout
```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int main(){
    int a, b;
    cin >> a >> b;
    cout << "a+b=" << a + b << endl;//endl是换行的意思，可连续使用
    return 0;
}
```
## scanf,printf
```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int main(){
    double a, b;
    scanf("%lf%lf", &a, &b);
    printf("%.2lf", a / b);
    return 0;
}
```
## 数组
```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int main(){
    int a[6];
    a[0] = 34;//=这个是赋值符号，不是等号，前面都忘记说了
    a[1] = 42;
    a[2] = 6;
    a[3] = 1;
    a[4] = 2;
    a[5] = 23;
    return 0;
}
```
![](https://pic.downk.cc/item/5f5720c7160a154a6794dd09.png)
也可以这样赋值
```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int main(){
    int a[6] = {34, 42, 6, 1, 2, 23};
    return 0;
}
```
## char
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    char a;
    cin >> a;//只能存一个，eg：输入abcd
    cout << a;//输出a
}
```
但是看这个情况
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    char a = 'acbd';//用单引号赋值
    cin >> a;
    cout << a;//输出d
}
```
### ASCII码表
[百度百科](https://baike.baidu.com/item/ASCII/309296?fr=aladdin)
为什么放在char这里将呢，看代码就懂了
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    char a = 'a';
    cout << (int)a;//这个叫强制类型转换，即强制将char（字符类型）转换成int（整数型）
    return 0;
}
```
输出结果是：`97`
对照ASCII表，小写字母a对应着的十进制数正好就是97，这是巧合吗？
同样的我们这样操作一下
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    int a = 97;
    cout << (char)a;
    return 0;
}
```
输出结果是：`a`
出现这样的结果是因为C++语言的字符函数设计是基于这个ASCII表的，过多的不赘述，目前也不要求掌握

## string
**注意要引入cstring的库**
string没有字符长度的限制，使用cin读入。以空格或者换行结束读入
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
int main(){
    string a;
    cin >> a;
    cout << a;
    return 0;
}
```
Input：`hello world`
Output：`hello`
## 判断语句
### **if else**
先理解一下基础
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    int a;
    cin >> a;
    if (a == 1)
    {
        cout << "a的值为1" << endl;
    }
    else cout << "a的值不为1" << endl;
    return 0;
}
```
扩展
`if (a <= 1)`
`if (a >= 1)`
`if (a != 1)`
`if (a > 1 && a <= 10)`
`if (a > 1 || a <0)`
`if (is_prime(a))`这个`is_prime`是一个自己写的函数的名称，现在只需要理解为判断a是否是质数，如果是质数，这条if语句就会得到一个允许进入的值，然后执行`{}`里面的内容
`...`
### switch（不是很重要）
```cpp
#include <iostream>
using namespace std;
 
int main ()
{
   // 局部变量声明
   char grade = 'D';
 
   switch(grade)
   {
        case 'A' :
            cout << "很棒！" << endl; 
        break;
        
        case 'B' :
            cout << "做的好！" << endl;
        case 'C' :
            cout << "还不错" << endl;
        break;
        
        case 'D' :
            cout << "您通过了" << endl;
        break;
        
        case 'F' :
            cout << "最好再试一次" << endl;
        break;
        
        default :
            cout << "无效的成绩" << endl;
   }
   cout << "您的成绩是 " << grade << endl;
 
   return 0;
}
```
梦幻联动前面的知识点，不过多解释，读懂就行

## bool
bool的值只有两个：
`1`和`0`
其中`1`也代表`true`,`0`代表`false`
请通过一下代码来理解bool的逻辑
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    bool a = true;
    cout << a;//输出结果为1,如果a=fasle，那么输出结果为0
    return 0;
}
```
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    bool a = 3;
    cout << a;//输出结果仍然为1
    return 0;
}
```
也就是说，除了`0`以外，其他所有的数在bool这个人面前都是`true`
只有`0`，在它面前是`fasle`

现在我们回想一下主函数里面的`return 0;`语句
它的意义应该是，返回一个0给操作系统 表示我的程序已经正常结束
这也就是竞赛系统中一定要保证自己主函数的返回值是0的原因了
但是这并不影响代码在项目中的运行（不必深究）
## 循环语句
### for

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    int a[6] = {12, 23, 34, 54, 65, 76};
    for (int i = 0; i < 6; i ++)
    {
        cout << "i = " << i << endl << "a[i] = " << a[i] << endl;
    }

    return 0;
}
```

```flow
st=>start: Start
op=>operation: int i = 0;
cond=>condition: 是否满足i<6
op2=>operation: 执行输出代码
op3=>operation: 更新循环控制变量
ed=>end: 退出循环语句
st->op->cond
cond(no)->ed
cond(yes)->op2->op3->cond
```

仔细理解一下过程，一定要掌握的
另外，`for(;;)`里面的三条语句并不是一定都要写的
实践一下如果不写其中某一条或几条会有什么结果

### while

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    int a[6] = {12,23,34,54,65,76};
    int i = 0;
    while(i < 6)
    {
        cout << "a["<< i <<"] = " << a[i] << endl;
        i++;
    }
    return 0;
}
```

```flow
st=>start: 开始
cond=>condition: 满足i<6
op=>operation: 执行输出语句
ed=>end: 退出循环
st->cond
cond(yes)->op->cond
cond(no)->ed
```

### do...while

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    int a[6] = {12,23,34,54,65,76};
    int i = 0;
    do
    {
        cout << a[i]<<endl;
        i++;
    }while(i < 6);

    return 0;
}
```

这个就不写流程图了emmm，高中知识相关

**三种循环语句的区别以及适用场景请大概品一品，目前不要求**

### break和continue
字面意思，很好理解
这些是用在循环语句中的“控制器”
**首先都是且只能是用在循环体中**
break是中止语句，到了这一步就会中止循环体并退出
continue是跳过的意思，到了这一步就会直接跳过后面的语句，开始下一次的循环
请看实例理解
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
    int a[6] = {12,23,34,54,65,76};
    int i = 0;
    for (int i = 0; i < 6; i++)
    {
        if (a[i] == 54)
        {
            cout << "该数组中存在值为54的元素" << endl;
            break;//已经找到了，不用再继续找了,会浪费时间
        }
        if (i == 5)
        {
            cout << "该数组中不存在值为54的元素" << endl;
        }
    }
    return 0;
}
```
输出结果：`该数组中存在54的数值，且位于数组中的3位置`

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
bool is_odd(int a){
    return a % 2 == 0 ? false : true;//三目运算符，先不用管
}
int main(){
    int a[6] = {12,23,34,54,65,76};
    int i = 0;
    for (int i = 0; i < 6; i++)
    {
        if (is_odd(a[i])) continue;//目前只需要理解is_odd(a[i])是判断a[i]是否为奇数的函数，如果是奇数，返回值就为true
        cout << a[i] << "是该数组中的偶数" << endl;
    }
    return 0;
}
```

输出结果：
```
12是该数组中的偶数
34是该数组中的偶数
54是该数组中的偶数
76是该数组中的偶数
```
## 函数
## 指针 和 引用
## struct
## STL重头戏

# 详解算法基本概念
## 图论
### 图
### 树
## 数论
### 欧几里得（gcd）
### 扩展欧几里得（exgcd）
### 线性筛，欧拉筛
## 搜索
### DFS
### BFS


**PS：计划失败，不更了**
