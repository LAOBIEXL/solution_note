# P1216 [IOI 1994 / USACO1.5] 数字三角形 Number Triangles

## 题目描述

观察下面的数字金字塔。


写一个程序来查找从最高点到底部任意处结束的路径，使路径经过数字的和最大。每一步可以走到左下方的点也可以到达右下方的点。

![](https://cdn.luogu.com.cn/upload/image_hosting/95pzs0ne.png)

在上面的样例中，从 $7 \to 3 \to 8 \to 7 \to 5$ 的路径产生了最大权值。

## 输入格式

第一个行一个正整数 $r$，表示行的数目。

后面每行为这个数字金字塔特定行包含的整数。

## 输出格式

单独的一行，包含那个可能得到的最大的和。

## 输入输出样例 #1

### 输入 #1

```
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5
```

### 输出 #1

```
30
```

## 说明/提示

【数据范围】  
对于 $100\%$ 的数据，$1\le r\le 1000$，所有输入在 $[0,100]$ 范围内。

题目翻译来自 NOCOW。

IOI1994 Day1T1 / USACO Training Section 1.5。

## 分析：
```cpp
1. 状态分析
f[i][j] 表示从1,1，走到i， j所用的最大的权值；

2. 状态转移方程：
f[i][j] = max(f[i-1][j-1], f[i-1][j]) + f[i][j];

3. 初始化：
```
这个地方特别强调一下，由于有部分的数值存在越界访问，==但是由于我们从一开始构建下标的习惯，所以越界访问的情况完全不用处理捏；==

```cpp
但是需要注意的是，需要将最外面一层初始化为零，防止在取值的时候取到这个原本不存在的值；

如果存在负数，直接全部初始化为负无穷大即可：

memset(f,-0x3f,sizeof(f))
如果有负数的话，其次还要单独处理第一个格子，还有把

f[0][0] = f[0][1] = 0;

4. 填表顺序：从上往下；
   
5. 输出结果：最后一行的最大值：
```

## 代码：
```cpp
#include<iostream>
using namespace std;

const int N = 1010;

int arr[N][N];
int f[N][N]; //f[N][N]表示从初始位置到这一个为止的最大值；

int r;

int main()
{
	cin >> r;
	for (int i = 1; i <= r; i++)
	{
		for (int j = 1; j <= i; j++)
		{
			cin >> arr[i][j];
		}
	}
	for (int i = 1; i <= r; i++)
	{
		for (int j = 1; j <= i; j++) {
			f[i][j] = max(f[i - 1][j - 1], f[i - 1][j]) + arr[i][j];
		}
	}
	int ret = 0;
	for (int i = 1; i <= r; i++)
	{
		ret = max(ret, f[r][i]);
	}
	cout << ret << endl;

	return 0;
}

```

### 二维滚动数组：
```cpp
#include<iostream>
using namespace std;

const int N = 1010;
int arr[N][N], f[N];//只保留一维，一维时候要注意填数的顺序；
int ret = 0;
int r;

int main()
{
	cin >> r;
	for (int i = 1; i <= r; i++)
	{
		for (int j = 1; j <= i; j++)
		{
			cin >> arr[i][j];
		}
	}
	for (int i = 1; i <= r; i++)
	{
		for (int j = i; j >= 1; j--)//反过来遍历
		{
			f[j] = max(f[j - 1], f[j]) + arr[i][j];
		}
	}
	for (int j = 1; j <= r; j++)
	{
		ret = max(ret, f[j]);
	}
	cout << ret << endl;


	return 0;
}

```

二维转一维：
1. 是否需要修改顺序；
2. 直接删掉一维即可；