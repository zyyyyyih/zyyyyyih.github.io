---
title: DFS+BFS
tags:
  - C++
  - 算法
categories:
  - 学习
abbrlink: dfsbfs
date: 2021-01-08 18:41:49
---
**两个非常基础且使用范围很广的基本思想**

这其中是通过一些题目来帮助理解和应用

>这两个算法很活的，多思考原理

<!-- more -->

# DFS（深度优先搜素）
我们设想这样一个问题，现在有一个可以称作迷宫的地图，我们要判断这个迷宫是否有从起点到终点的路径
假设现在有一个6*7的地图，那么我们的任务就是判断
我们能否从第一行第一列即（1，1），走到（6，7）
如下图
![](https://gitee.com/zyyyyyih/picture-bed/raw/master/img/20210108185229.png)
数据如下
```
6 7
0 0 1 1 1 0 1
1 0 1 0 0 0 1
1 0 0 1 0 1 1
1 1 0 0 0 0 1
1 1 1 1 0 1 1
1 1 1 1 0 0 0
```
当我们使用DFS解决这个问题时，思路应当是
1. 以（1，1）作为起点，我们对坐标进行操作，得到它相邻点的坐标
2. 判断这个相邻的点能否作为下一个落脚点，即能不能走
3. 走到这个点之后，再以这个点作为起点，进行同样的操作
4. 判断这整个遍历过程中是否存在走到了（6，7）这个点的情况，如果存在，那么这个地图有解，反之无解。
于是可以比较容易的得到如下代码
```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
const int MAXN = 1000;
const int MAXM = 1000;
int n, m, f[MAXN][MAXM], vis[MAXN][MAXM], ans;//其实全局变量默认是0
void dfs(int a, int b){
    vis[a][b] = 1;//标记为访问过
    if (a == n && b == m)
    {
        ans = 1;
        return ;//找到终点，可以直接结束，这个return其实也可以不要
    }
    if (!f[a + 1][b] && a + 1 <= n && !vis[a + 1][b]) dfs(a + 1, b);//判断是否是路，是否越界，是否访问过
    if (!f[a][b + 1] && b + 1 <= n && !vis[a][b + 1]) dfs(a, b + 1);
    if (!f[a - 1][b] && a - 1 >= 1 && !vis[a - 1][b]) dfs(a - 1, b);
    if (!f[a][b - 1] && b - 1 >= 1 && !vis[a][b - 1]) dfs(a, b - 1);
}
int main(){
    cin >> n >> m;
    memset(vis, 0, sizeof(vis));//初始化，可不要
    for (int i = 1; i <= n; i ++)
        {
            for (int j = 1; j <= m; j ++)
            cin >> f[i][j];
        }
    dfs(1, 1);
    if (ans == 1) cout << "yes";
    else cout << "no";
    return 0;
}
```
请理解后，去尝试一下这到经典的DFS题目[八皇后](https://www.luogu.com.cn/problem/P1219)

# BFS（广度优先搜索）
## DFS可以代替吗
我们直接看到这道题[填涂颜色](https://www.luogu.com.cn/problem/P1162)

![](https://gitee.com/zyyyyyih/picture-bed/raw/master/img/20210108190437.png)
样例数据
**INPUT**
```
6
0 0 0 0 0 0
0 0 1 1 1 1
0 1 1 0 0 1
1 1 0 0 0 1
1 0 0 0 0 1
1 1 1 1 1 1
```
**OUTPUT**
```
0 0 0 0 0 0
0 0 1 1 1 1
0 1 1 2 2 1
1 1 2 2 2 1
1 2 2 2 2 1
1 1 1 1 1 1
```
来分析一下题目，如果这道题用dfs怎么做
1. 我们可以找到一个0，并且以它为起点，然后通过dfs去标记所有相连接的0
2. 再找到一个1，以它为起点，去标记所有相连接的1
3. 最后再把所有未标记的点的数值变为2

很容易发现，这样的想法有很多的问题，假如外围的0并不相接呢，是不是需要多个起点才能解决问题

又或者，其他的想法，比如先找到一个1去标记一圈，然后往中间找0，再全部改成2。

想法不错，可是这个代码的实现是不是很难完成，要进行各种判断才能确定这个0是在圈内的

所以我们来看一下BFS有什么解决办法
首先BFS的实现离不开队列，这里简单介绍一下队列
### <queue>队列简介
队列是一种数据结构，queue是属于C++函数库中的一个成熟的数据结构，即属于STL
通过头文件`#include <queue>`来引用
通过`queue<数据类型> 变量名`来定义
数据从队尾插入，从队首弹出
例如：
```cpp
queue<int>Q;//定义一个队列名称为Q的队列
Q.push(2);//在队列Q的队尾插入一个int类型的数据2
int a = Q.front();//定义一个变量a，并将队列Q中位于队首的元素赋值给a
Q.pop();//弹出队首的元素
if(Q.empty())//如果队列为空，Q.empty()的返回值为true，有元素返回fasle
```
下次详细讲解队列，（）留个链接的位置在这

## BFS怎么实现
我们来解析代码
```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <cstring>
using namespace std;
const int MAXN = 50;
struct NODE
{
	int x, y;
	NODE(int x, int y) : x(x), y(y) {}//初始化
};
int n, b[MAXN][MAXN], dx[4] = {0, 0, 1, -1}, dy[4] = {1, -1, 0, 0};//用这个来代替走的步子，看到后面就明白了
bool vis[MAXN][MAXN];
char a[MAXN][MAXN];
queue<NODE> que;//定义一个队列
bool judge(int x, int y)//判断这个点是否合法
{
	return x > 0 && y > 0 && x <= n && y <= n && !vis[x][y] && a[x][y] == 0;
}
inline void bfs()
{
	for (int i = 1; i <= n; i++)
	{
		if (a[1][i] == 0)
			que.push(NODE(1, i)), b[1][i] = 0, vis[1][i] = true;
		if (a[n][i] == 0)
			que.push(NODE(n, i)), b[n][i] = 0, vis[n][i] = true;
		if (a[i][1] == 0)
			que.push(NODE(i, 1)), b[i][1] = 0, vis[i][1] = true;
		if (a[i][n] == 0)
			que.push(NODE(i, n)), b[i][n] = 0, vis[i][n] = true;
	}//描边，将所有的外围的点扫描一遍，并放入队列中作为起点
	while (!que.empty())//算法关键
	{
		NODE cur = que.front();//取出队首的元素
		que.pop();//弹出这个元素
		for (int i = 0; i < 4; i ++)//通过for循环来简化代码，如果不用这个方法的话，就需要像我上面那个未简化的dfs的代码一样，把向上下左右走的情况都列出来
		{
			int nx = cur.x + dx[i], ny = cur.y + dy[i];//用nx和ny表示走的下一步格子
			if (judge(nx, ny))//这个格子是否合法
			{
				vis[nx][ny] = true;//标记为访问过
				b[nx][ny] = 0;//改回0
				que.push(NODE(nx, ny));//把这个点作为其他节点的起点加入队列中
			}
		}
	}//当队列为空时，就表示所有与起点相连接的点都遍历完了，就会弹空队列，就退出循环，完成了操作
}
int main()
{
	scanf("%d", &n);
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++)
		{
			scanf("%d", &a[i][j]);
			if (a[i][j] == 1)
				b[i][j] = 1;
			else
				b[i][j] = 2;//我这里是使用了一种小技巧，于算法本身无关，我是先把所有的零都处理成2，然后用bfs把圈外的还原成0
		}
	
	bfs();//调用函数主体

	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{

			printf("%d", b[i][j]);
			if (j != n)
				printf(" ");
		}
			printf("\n");
	}//输出不多说了
	return 0;
}
```
本题还有很多其他更加优秀的解法，这份代码会更加方便入门bfs，效率并不是最优的
练习[奇怪的电梯](https://www.luogu.com.cn/problem/P1135)