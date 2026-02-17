# P1443 马的遍历
[[搜索]]
## 题目描述
有一个 $n \times m$ 的棋盘，在某个点 $(x, y)$ 上有一个马，要求你计算出马到达棋盘上任意一个点最少要走几步。

## 输入格式

输入只有一行四个整数，分别为 $n, m, x, y$。

## 输出格式

一个 $n \times m$ 的矩阵，代表马到达某个点最少要走几步（不能到达则输出 $-1$）。

## 输入输出样例 #1

### 输入 #1

```
3 3 1 1
```

### 输出 #1

```
0 3 2    
3 -1 1    
2 1 4
```

## 说明/提示

### 数据规模与约定

对于全部的测试点，保证 $1 \leq x \leq n \leq 400$，$1 \leq y \leq m \leq 400$。

2022 年 8 月之后，本题去除了对输出保留场宽的要求。为了与之兼容，本题的输出以空格或者合理的场宽分割每个整数都将判作正确。

## 分析：
依旧方向向量；
bfs在写队列不为空的循环内，==核心是脱离第一个点独立运行==，即后续的遍历和入队都不能被函数头传入的第一个元素影响，否则一定会出错；
[[小技巧]]
==要注意的是，距离结果存储在dis数组中，由于零本身是一个有效的距离，所以应该初始化为-1，由于-1的特殊补码，使用memset很好用；


[[错误汇总]]
![[Pasted image 20251218224434.png]]
```cpp
queue.front() queue.back()
```

另外，矩阵题你习惯从零开始，但是题目给出的开始坐标是从一开始，所以一定要注意看要求；
![[Pasted image 20251218230723.png]]
## 代码：
```cpp
#include <iostream>

#include <queue>

#include <cstring>

using namespace std;

typedef pair<int, int> PII;

const int N = 410;

int n, m, x, y;

int dist[N][N];

int dx[] = {1, 2, 2, 1, -1, -2, -2, -1};

int dy[] = {2, 1, -1, -2, -2, -1, 1, 2};

void bfs()

{

memset(dist, -1, sizeof dist);

queue<PII> q;

q.push({x, y});

dist[x][y] = 0;

while(q.size())

{

auto t = q.front(); q.pop();

int i = t.first, j = t.second;

for(int k = 0; k < 8; k++)

{

int x = i + dx[k], y = j + dy[k];

if(x < 1 || x > n || y < 1 || y > m) continue;

if(dist[x][y] != -1) continue;

dist[x][y] = dist[i][j] + 1; // 更细结果

q.push({x, y});

}

}

}

int main()

{

cin >> n >> m >> x >> y;

bfs();

for(int i = 1; i <= n; i++)

{

for(int j = 1; j <= m; j++)

{

cout << dist[i][j] << " ";

}

cout << endl;

}

return 0;

}
```