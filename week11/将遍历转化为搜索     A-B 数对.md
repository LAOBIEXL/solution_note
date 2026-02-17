# P1102 A-B 数对
[[哈希表 和 模拟实现]]
## 题目背景

出题是一件痛苦的事情！

相同的题目看多了也会有审美疲劳，于是我舍弃了大家所熟悉的 A+B Problem，改用 A-B 了哈哈！

## 题目描述

给出一串正整数数列以及一个正整数 $C$，要求计算出所有满足 $A - B = C$ 的数对的个数（不同位置的数字一样的数对算不同的数对）。

## 输入格式

输入共两行。

第一行，两个正整数 $N,C$。

第二行，$N$ 个正整数，作为要求处理的那串数。

## 输出格式

一行，表示该串正整数中包含的满足 $A - B = C$ 的数对的个数。

## 输入输出样例 #1

### 输入 #1

```
4 1
1 1 2 3
```

### 输出 #1

```
3
```

## 说明/提示

对于 $75\%$ 的数据，$1 \leq N \leq 2000$。

对于 $100\%$ 的数据，$1 \leq N \leq 2 \times 10^5$，$0 \leq a_i <2^{30}$，$1 \leq C < 2^{30}$。

2017/4/29 新添数据两组

## 分析：
```cpp
A - B = C;
A = B + C;
所以对于每一个B，都检索A在整个数组中出现的次数，将遍历问题转化为搜索问题；
```

## 代码：
```cpp
#include<iostream>
#include<string>
#include<cstring>
#include<vector>
#include<set>
#include<map>
#include<unordered_map>
#include<unordered_set>
#include<algorithm>
#include<cmath>
#include<queue>
#include <deque>
#include <stack>
#include<iomanip>
#include <chrono>
#include<random>
using namespace std;
typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int, int> PII;
using namespace std;

const int N = 2e5 + 10;
LL a[N];


int main()
{
    LL N; LL C;
    cin >> N >> C;
    unordered_map<int, int > mp;
    for (int i = 1; i <= N; i++)
    {
        LL tmp;
        cin >> tmp;
        mp[tmp]++;
        a[i] = tmp;
    }
    LL sum = 0;
    for (int i = 1; i <= N; i++)
    {
        sum += mp[C + a[i]];
    }
    cout << sum << endl;


	return 0;
}
```