# P1102 A-B 数对
[[二分]]
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

```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
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
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int, int> PII;
const int N = 2e5 + 10;
ll a[N];
signed main()
{
	ll n, c;
	cin >> n >> c;
	for(int i = 0; i < n; i++)
	{
		cin >> a[i];
	}
	sort(a, a + n);
	ll ret = 0;
	for (int i = 0; i < n; i++)
	{
		ll t = a[i] - c;
		ll* pos1 = lower_bound(a, a + n, t);
		ll* pos2 = upper_bound(a, a + n, t);
		ret += pos2 - pos1;
	}
	cout << ret << endl;


}
```