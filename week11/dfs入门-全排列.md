# P1706 全排列问题
[[搜索]]
## 题目描述

按照字典序输出自然数 $1$ 到 $n$ 所有不重复的排列，即 $n$ 的全排列，要求所产生的任一数字序列中不允许出现重复的数字。

## 输入格式

一个整数 $n$。

## 输出格式

由 $1 \sim n$ 组成的所有不重复的数字序列，每行一个序列。

每个数字保留 $5$ 个场宽。

## 输入输出样例 #1

### 输入 #1

```
3
```

### 输出 #1

```
1    2    3
    1    3    2
    2    1    3
    2    3    1
    3    1    2
    3    2    1
```

## 说明/提示

$1 \leq n \leq 9$。

## 代码：
```cpp
#include <iostream>

#include <vector>

using namespace std;

const int N = 15;

int n;

bool st[N];

vector<int> path;

void dfs()

{

if(path.size() == n)

{

for(auto x : path)

{

printf("%5d", x);

}

cout << endl;

return;

}

for(int i = 1; i <= n; i++)

{

if(st[i]) continue;

path.push_back(i);

st[i] = true;

dfs();

// 恢复现场

path.pop_back();

st[i] = false;

}

}

int main()

{

cin >> n;

dfs();

return 0;

}
```