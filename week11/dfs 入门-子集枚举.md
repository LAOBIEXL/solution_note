# B3622 枚举子集（递归实现指数型枚举）
[[搜索]]
## 题目描述

今有 $n$ 位同学，可以从中选出任意名同学参加合唱。

请输出所有可能的选择方案。

## 输入格式

仅一行，一个正整数 $n$。

## 输出格式

若干行，每行表示一个选择方案。

每一种选择方案用一个字符串表示，其中第 $i$ 位为 `Y` 则表示第 $i$ 名同学参加合唱；为 `N` 则表示不参加。

需要以字典序输出答案。

## 输入输出样例 #1

### 输入 #1

```
3
```

### 输出 #1

```
NNN
NNY
NYN
NYY
YNN
YNY
YYN
YYY
```

## 说明/提示

对于 $100\%$ 的数据，保证 $1\leq n\leq 10$。

## 分析：
首先，要求字典序输出，使用dfs;

## 代码：
```cpp
#include <iostream>

using namespace std;

int n;

string path; // 记录递归过程中，每⼀步的决策

void dfs(int pos)

{

if(pos > n)

{

// path 就存着前 n 个⼈的决策

cout << path << endl;

return;

}

// 不选

path += 'N';

dfs(pos + 1);

path.pop_back(); // 回溯，清空现场

// 选

path += 'Y';

dfs(pos + 1);

path.pop_back(); // 清空现场

}

int main()

{

cin >> n;

dfs(1);

return 0;

}
```