# P3405 [USACO16DEC] Cities and States S
[[哈希表 和 模拟实现]]
## 题目描述

Farmer John 有若干头奶牛。为了训练奶牛们的智力，Farmer John 在谷仓的墙上放了一张美国地图。地图上表明了每个城市及其所在州的代码（前两位大写字母）。

由于奶牛在谷仓里花了很多时间看这张地图，他们开始注意到一些奇怪的关系。例如，FLINT 的前两个字母就是 MIAMI 所在的 `FL` 州，MIAMI 的前两个字母则是 FLINT 所在的 `MI` 州。  
确切地说，对于两个城市，它们的前两个字母互为对方所在州的名称。

我们称两个城市是一个一对「特殊」的城市，如果他们具有上面的特性，并且来自不同的州。对于总共 $N$ 座城市，奶牛想知道有多少对「特殊」的城市存在。请帮助他们解决这个有趣的地理难题！

## 输入格式

输入共 $N + 1$ 行。

第一行一个正整数 $N$，表示地图上的城市的个数。  
接下来 $N$ 行，每行两个字符串，分别表示一个城市的名称（$2 \sim 10$ 个大写字母）和所在州的代码（$2$ 个大写字母）。同一个州内不会有两个同名的城市。

## 输出格式

输出共一行一个整数，代表特殊的城市对数。

## 输入输出样例 #1

### 输入 #1

```
6
MIAMI FL
DALLAS TX
FLINT MI
CLEMSON SC
BOSTON MA
ORLANDO FL
```

### 输出 #1

```
1
```

## 说明/提示

### 数据规模与约定

对于 $100\%$ 的数据，$1 \leq N \leq 2 \times 10 ^ 5$，城市名称长度不超过 $10$。

## 分析：
1. 所求是多少对城市，我们基本上会使用哈希表来解决，如果
计算a--b  再计算b--a,就相当于算了两遍，这样的话就需要除以2；

所以，如果每次读入一个城市，就判断unordered_map中是否存储了相应的城市，然后计算完之后再把这个城市加入到unordered_map中，这样计算就不会有重复；

2. 城市的前两个字母和州的名字不能相同，所以如果读取到州名字和城市名字相同的城市，直接跳过即可，所有这种类型的城市都不会被计算在内；

 注意了，使用哈希表的时候，unordered_map对很多种情况没有提供哈希函数，所以只能使用map替代；或者缩略信息，比如将含有两个string的pair合并为一个string;
 ```cpp
   int n;
  cin >> n;
  int t = n;
  unordered_map<pair<string,string>, int> mp;
  while (n--)
  {
      string a, b;
      cin >> a >> b;
      a = a.substr(0, 2);
      if (a == b) continue;
      mp[{a,b}]++;
  }
  int ret = 0;
  for (auto e : mp)
  {
      int tmp = e.second;
      string a1 = e.first.first;
      string a2 = e.first.second;

  }
 ```
![[哈希.jpg]]
## 代码：
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
using namespace std;
const int N = 0;


int main()
{
    int n;
    cin >> n;
    int t = n;
    map<pair<string,string>, int> mp;
    while (n--)
    {
        string a, b;
        cin >> a >> b;
        a = a.substr(0, 2);
        if (a == b) continue;
        mp[{a,b}]++;
    }
    int ret = 0;
    for (auto e : mp)
    {
        int tmp = e.second;
        string a1 = e.first.first;
        string a2 = e.first.second;
        if (mp.count({ a2,a1 }))  ret += (mp[{a2, a1}]) * tmp;
    }
    cout << ret/2 << endl;





    return 0;
}

```
![[Pasted image 20251203142158.png]]
