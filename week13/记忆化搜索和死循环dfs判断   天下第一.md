# P5635 【CSGRound1】天下第一
[[搜索]]
## 题目背景

天下第一的 cbw 以主席的身份在 8102 年统治全宇宙后，开始了自己休闲的生活，并邀请自己的好友每天都来和他做游戏。由于 cbw 想要显出自己平易近人，所以 zhouwc 虽然是一个蒟蒻，也有能和 cbw 玩游戏的机会。

## 题目描述

游戏是这样的：

给定两个数 $x$，$y$，与一个模数 $p$。

cbw 拥有数 $x$，zhouwc 拥有数 $y$。

第一个回合：$x\leftarrow(x+y)\bmod p$。

第二个回合：$y\leftarrow(x+y)\bmod p$。

第三个回合：$x\leftarrow(x+y)\bmod p$。

第四个回合：$y\leftarrow(x+y)\bmod p$。

以此类推....

如果 $x$ 先到 $0$，则 cbw 胜利。如果 $y$ 先到 $0$，则 zhouwc 胜利。如果 $x,y$ 都不能到 $0$，则为平局。

cbw 为了捍卫自己主席的尊严，想要提前知道游戏的结果，并且可以趁机动点手脚，所以他希望你来告诉他结果。

## 输入格式

有多组数据。

第一行：$T$ 和 $p$ 表示一共有 $T$ 组数据且模数都为 $p$。

以下 $T$ 行，每行两个数 $x,y$。

## 输出格式

共 $T$ 行

$1$ 表示 cbw 获胜，$2$ 表示 zhouwc 获胜，```error``` 表示平局。

## 输入输出样例 #1

### 输入 #1

```
1 10
1 3
```

### 输出 #1

```
error
```

## 输入输出样例 #2

### 输入 #2

```
1 10
4 5
```

### 输出 #2

```
1
```

## 说明/提示

$1 \leq T \leq 200$。

$1 \leq x,y,p \leq 10000$。


## 分析：
==这个题目再次显示出了记忆化搜索的出现条件，对于每T组测试样例，我们都有同样的p，也就是说可以通过存储值来大大简化这一组全部样例的计算时间；==
[[小技巧]]
此外，注意到数据的范围是1 - 10000，如果开启数组存储记忆值，会导致空间超出；
==但是，本题的返回值只是A成功B成功和失败三种状态，所以大可开一个一万乘一万的字符数组，并且用简单字符表示结果；

最后，本题需要判断有==没有发生循环，返回状态三==，这个点需要学习一下；

## 代码：
```cpp
map版本
**#include<iostream>
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
const int N = 1e4 +10;
#define endl '\n'
int p;
int x, y;

map<PII, int> mp;

int dfs(int x, int y)
{
    if (mp.count({x, y})) return mp[{x, y}];
    if (x == 0) return 1;
    if (y == 0) return 2;
    mp[{x, y}] = 3;
    return mp[{x, y}] = dfs((x + y) % p, ((x + 2 * y) % p));
}


void solve()
{
    //不要忘记清空数组；
    cin >> x >> y;
    int ret = dfs(x, y);
    if (ret == 1) cout << 1 << endl;
    else if (ret == 2) cout << 2 << endl;
    else if (ret == 3) cout << "error" << endl;

}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int TestCase = 1;
    cin >> TestCase >> p;
    while (TestCase--)
        solve();
}
**
```
```cpp
#include <iostream>

using namespace std;

const int N = 1e4 + 10;

int x, y, p;

char f[N][N]; // 备忘录

char dfs(int x, int y)

{

if(f[x][y]) return f[x][y]; // 剪枝

f[x][y] = '3'; // 这个状态已经访问过了，之后再遇到时，表⽰平局

if(x == 0) return f[x][y] = '1';

if(y == 0) return f[x][y] = '2';

return f[x][y] = dfs((x + y) % p, (x + y + y) % p);

}

int main()

{

int T; cin >> T >> p;

while(T--)

{

cin >> x >> y;

char ret = dfs(x, y);

if(ret == '1') cout << 1 << endl;

else if(ret == '2') cout << 2 << endl;

else cout << "error" << endl;

}

return 0;

}


```