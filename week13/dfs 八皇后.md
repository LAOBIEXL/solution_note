# P1219 [USACO1.5] 八皇后 Checker Challenge
[[搜索]]
## 题目描述

一个如下的 $6 \times 6$ 的跳棋棋盘，有六个棋子被放置在棋盘上，使得每行、每列有且只有一个，每条对角线（包括两条主对角线的所有平行线）上至多有一个棋子。

![](https://cdn.luogu.com.cn/upload/image_hosting/3h71x0yf.png)

上面的布局可以用序列 $2\ 4\ 6\ 1\ 3\ 5$ 来描述，第 $i$ 个数字表示在第 $i$ 行的相应位置有一个棋子，如下：

行号 $1\ 2\ 3\ 4\ 5\ 6$

列号 $2\ 4\ 6\ 1\ 3\ 5$

这只是棋子放置的一个解。请编一个程序找出所有棋子放置的解。  
并把它们以上面的序列方法输出，解按字典顺序排列。  
请输出前 $3$ 个解。最后一行是解的总个数。

## 输入格式

一行一个正整数 $n$，表示棋盘是 $n \times n$ 大小的。

## 输出格式

前三行为前三个解，每个解的两个数字之间用一个空格隔开。第四行只有一个数字，表示解的总数。

## 输入输出样例 #1

### 输入 #1

```
6
```

### 输出 #1

```
2 4 6 1 3 5
3 6 2 5 1 4
4 1 5 2 6 3
4
```

## 说明/提示

【数据范围】  
对于 $100\%$ 的数据，$6 \le n \le 13$。
      
题目翻译来自NOCOW。

USACO Training Section 1.5


## 分析：
==判断对角线上是否已经存在元素的方法：==
[[坐标系技巧]][[哈希表 和 模拟实现]]
依旧使用哈希表的思路，哈希函数就是解析式，将对应的横坐标和纵坐标带入后的值就是下标，有两个注意的细节：值可能是负数，所以需要加上一个数来补偿  [[小技巧]]，对角线的个数多于元素的个数，所以需要开两倍的大小；
![[1765882775462.png]]

![[Pasted image 20251216192904.png]]
## 代码：
```cpp
#include <iostream>

#include <vector>

using namespace std;

const int N = 15;

int n;

bool col[N], st1[N * 2], st2[N * 2];

int ret;

vector<int> path;

void dfs(int x)

{

if(x > n)

{

ret++;

if(ret <= 3)

{

for(auto x : path) cout << x << " ";

cout << endl;

}

return;

}

for(int y = 1; y <= n; y++)

{

// 判断能不能摆在这⼀列

if(col[y] || st1[y - x + n] || st2[y + x]) continue; // 剪枝

col[y] = st1[y - x + n] = st2[y + x] = true;

path.push_back(y);

dfs(x + 1);

col[y] = st1[y - x + n] = st2[y + x] = false;

path.pop_back();

}

}

int main()

{

cin >> n;

dfs(1);

cout << ret << endl;

return 0;

}
```