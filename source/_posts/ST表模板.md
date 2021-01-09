---
title: ST表模板
tags: C++
categories: 学习
abbrlink: eeaad1e3
date: 2020-08-15 18:24:16
---
# ST表
ST表的功能很简单

它是解决RMQ问题(区间最值问题)的一种强有力的工具

它可以做到$O(nlogn)$预处理，$O(1)$查询最值

# 算法
ST表是利用的是倍增的思想

拿最大值来说

我们用$Max[i][j]$表示，从$i$位置开始的$2^j$个数中的最大值，例如$Max[i][1]$表示的是$i$位置和$i+1$位置中两个数的最大值

那么转移的时候我们可以把当前区间拆成两个区间并分别取最大值（注意这里的编号是从$1$开始的）
查询的时候也比较简单
我们计算出$log_2$区间长度

然后对于左端点和右端点分别进行查询，这样可以保证一定可以覆盖查询的区间

刚开始学的时候我不太理解为什么从右端点开始查的时候左端点是$r-2^k+1$

实际很简单，因为我们需要找到一个点$x$，使得$x+2^{k}-1=d$

这样的话就可以得到$x=r-2^k+1$

# 代码
```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
using namespace std;
const int N = 100000 + 10;
int n, m, f[N][30], p[N], len, a[N];
inline int read(){
	int x = 0, f = 1;
	char ch = getchar();
	while (ch < '0' || ch > '9') {if (ch == '-') f = -1; ch = getchar();}
	while (ch >= '0' && ch <= '9') {x = x * 10 + ch -'0'; ch = getchar();}
	return x * f;
}
int main(){
	n = read();
	m = read();
	len = (int) (log(n) / log(2));//向下取整
	for (int i = 1; i <= n; i ++)
	{
		a[i] = read();
	}
	for (int i = 1; i <= n; i ++)
	{
		f[i][0] = a[i];
	}
	p[0] = 1;
	for (int i = 1; i <= len; i ++)
	{
		p[i] = p[i - 1] << 1;
	}
	for (int i = 1; i <= len; i ++)
	{
		for (int j = 1; j <= n - p[i] + 1; j ++)
		{
			f[j][i] = max(f[j][i - 1], f[j + p[i - 1]][i - 1]);
		}
	}
	
	while (m--)
	{
		int l , r;
		l = read();r = read();
		int q = (int) (log(r - l + 1) / log(2));
		printf("%d\n", max(f[l][q], f[r - p[q] + 1][q]));//左闭右开区间
	}
	return 0;
}
```
