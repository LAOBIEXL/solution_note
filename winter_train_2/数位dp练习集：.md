[[数位dp]]

#### 其他进制的数位dp
![[Pasted image 20260219170646.png]]

K个互不相等的B的整数次幂之和：
即某个数的B进制表示中，是否恰好有K个1；

```cpp
#include <iostream>
#include <cstring>

using namespace std;
const int N = 35;

int n;
int a[N];
int f[N][N][2];
int k, b;

int dfs(int pos, int sum, bool limit) {
    if (~f[pos][sum][limit]) {
        return f[pos][sum][limit];
    }

    if (pos == 0) {
        if (sum == k) {
            return 1;
        }
    }

    int ret = 0;


    int up = limit ? min(a[pos], 1) : 1;

    for (int i = 0; i <= up; i++) {
        ret += dfs(pos - 1, sum + i, limit && (i == a[pos]));
    }

    return f[pos][sum][limit] = ret;

}


int calc(int d) {
    memset(f, -1, sizeof(f));
    int n = 0;

    while (d) {
        a[++n] = d % b;
        d /= b;
    }

    return dfs(n, 0, 1);
}

int main() {
    int x, y;
    cin >> x >> y;

    cin >> k >> b;

    cout << calc(y) - calc(x - 1) << endl;



    return 0;
}
```

#### 练习2：
![[Pasted image 20260219185735.png]]
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
const int N = 1000;
#define endl '\n'

int k = 0;
int n;
int a[N];
int f[N][N];

int dfs(int pos, int su, bool limit)
{
    if (!limit && ~f[pos][su])
    {
        return f[pos][su];
    }
    if (pos == 0)
    {
        if (!su)
        {
            return 1;
        }
        else
        {
            return 0;
        }
    }
    int ret = 0;
    int up = limit ? a[pos] : 9;
    for (int i = 0; i <= up; i++)
    {
        ret += dfs(pos - 1, (su + i) % k, limit && a[pos] == i);
    }
    if (!limit)
    {
        f[pos][su] = ret;
    }
    return ret;
}

int calc(int d)
{
    memset(f, -1, sizeof(f));
    int n = 0;

    while (d)
    {
        a[++n] = d % 10;
        d /= 10;
    }
    return dfs(n, 0, 1);
}

void solve()
{
    //不要忘记清空数组；
    //你个nt递归死路记得也要写返回；
    int a, b;

    while (cin >> a >> b >> k)
    {
        cout << calc(b) - calc(a - 1) << endl;
    }
}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int TestCase = 1;
 
    while (TestCase--)
        solve();
}

```

#### 练习3 ： 

![[Pasted image 20260219212227.png]]

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
const int N = 1000;
#define endl '\n'

int n;
int f[N][N];
int a[N];

int dfs(int pos, int prev, bool limit)
{
    if (!limit && ~f[pos][prev])
    {
        return f[pos][prev];
    }
    if (pos == 0)
    {
        return 1;
    }
    int ret = 0;
    int up = limit ? a[pos] : 9;
    for (int i = 0; i <= up; i++)
    {
        if (i == 4) continue;
        if (prev == 6 && i == 2) continue;
        ret += dfs(pos - 1, i, limit && a[pos] == i);
    }
    if (!limit)
    {
        f[pos][prev] = ret;
    }
    return ret;
}

int calc(int d)
{
    n = 0;
    memset(f, -1, sizeof(f));
    while (d)
    {
        a[++n] = d % 10;
        d /= 10;
    }
    return dfs(n, 0, 1);
}

void solve()
{
    //不要忘记清空数组；
    //你个nt递归死路记得也要写返回；
    int A, b;
    while (true)
    {
        cin >> A >> b;
        if (A == 0 && b == 0)
        {
            break;
        }
        cout << calc(b) - calc(A- 1) << endl;
    }
}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int TestCase = 1;
    while (TestCase--)
        solve();
}
```

#### 数字计数：
![[Pasted image 20260219215858.png]]
**本题的重点**：
在递归中向下传递参数，考虑前导零，多个问题如何分组解决：
1. 递归中向下传递参数：
如果只是每次在for循环中出现一次目标数字就加一，相当于在高位分叉点计算了一次，所有分支都没有计算，计算的数量会少非常多；
所以，使用sum变量，在每一条链路上传递，最后复位，这是搜索算法的迁移运用；

2. 前导零的处理
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
const int N = 200;
#define endl '\n'

int d;
int a[N];
int n;
ll f[N][N];

ll dfs(int pos, ll sum,bool zero,  bool limit)
{
    if (!zero && !limit && ~f[pos][sum])
    {
        return f[pos][sum];
    }
    if (pos == 0)
    {
        return sum;
    }
    ll ret = 0;
    int up = limit ? a[pos] : 9;
    for (int i = 0; i <= up; i++)
    {
        int t = (d == i);
        if (zero && !i) t = 0;
        ret += dfs(pos - 1, sum + t, zero && !i, limit && a[pos] == i);
    }
    if (!limit && !zero)
    {
        f[pos][sum] = ret;
    }
    return ret;
}

ll calc(ll v)
{
    memset(f, -1, sizeof(f));
    int n = 0;
    while (v)
    {
        a[++n] = v % 10;
        v /= 10;
    }
    return dfs(n, 0, 1, 1);
}


void solve()
{
    //不要忘记清空数组；
    //你个nt递归死路记得也要写返回；
    ll x, y;
    cin >> x >> y;
    for (d = 0; d <= 9; d++)
    {
        cout << calc(y) - calc(x - 1) << " ";
    }
}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int TestCase = 1;
    while (TestCase--)
        solve();
}
```

#### 再探digit folding:
[[G digit folding]]