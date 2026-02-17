# P1588 [USACO07OPEN] Catch That Cow S

## 题目描述

FJ 丢失了他的一头牛，他决定追回他的牛。已知 FJ 和牛在一条直线上，初始位置分别为 $x$ 和 $y$，假定牛在原地不动。FJ 的行走方式很特别：他每一次可以前进一步、后退一步或者直接走到 $2\times x$ 的位置。计算他至少需要几步追上他的牛。

## 输入格式

第一行为一个整数 $t\ ( 1\le t\le 10)$，表示数据组数；

接下来每行包含一个两个正整数 $x,y\ (0<x,y \le 10^5)$，分别表示 FJ 和牛的坐标。

## 输出格式

对于每组数据，输出最少步数，每组数据间用换行隔开。

## 输入输出样例 #1

### 输入 #1

```
1 
5 17
```

### 输出 #1

```
4
```

## 分析：
```cpp
#include <iostream>

#include <queue>

#include <cstring>

using namespace std;

const int N = 1e5 + 10;

int n = 1e5;

int x, y;

int dist[N];

void bfs()

{

queue<int> q;

q.push(x);

dist[x] = 0;

while(q.size())

{

auto t = q.front(); q.pop();

int a = t + 1, b = t - 1, c = t * 2;

if(a <= n && dist[a] == -1)

{

dist[a] = dist[t] + 1;

q.push(a);

}

if(b > 0 && dist[b] == -1)

{

dist[b] = dist[t] + 1;

q.push(b);

}

if(c <= n && dist[c] == -1)

{

dist[c] = dist[t] + 1;

q.push(c);

}
// 剪枝

if(a == y || b == y || c == y) return;

}

}

int main()

{

int T; cin >> T;

while(T--)

{

// 注意清空数据

memset(dist, -1, sizeof dist);

cin >> x >> y;

bfs();

cout << dist[y] << endl;

}

return 0;

}
```