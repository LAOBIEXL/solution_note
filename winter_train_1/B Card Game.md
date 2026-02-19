[[贪心]]
![[Pasted image 20260217144904.png]]
这题的核心是这不是田忌赛马的问题，b的小手牌在对比后是不会发生变化的，所以会一直存在直到遇到更小的牌为止；

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
const int N = 0;
#define endl '\n'

ll cal(ll x)
{
    int  n = 1;
    for (int i = 1; i <= x; i++)
    {
        n = n * i % 998244353;
    }
    return n;
}


void solve()
{
    //不要忘记清空数组；
    ll n;
    cin >> n;
    vector<ll> a(n);
    vector<ll> b(n);
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
    }
    ll minb = 0x3f3f3f3f;
    for (int i = 0; i < n; i++)
    {
        cin >> b[i];
        minb = min(minb, b[i]);
    }
    ll cnt = 0;
    for (int i = 0; i < n; i++)
    {
        if (a[i] > minb)
        {
            cnt++;
        }
    }
    ll cntp = n - cnt;
    ll ret = (cal(cnt) * cal(cntp)) % 998244353;
    cout << ret << endl;
}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int TestCase = 1;
    cin >> TestCase;
    while (TestCase--)
        solve();
}
```

[[错误汇总]]
```cpp
ll cal(ll x)
{
    int  n = 1;
    for (int i = 1; i <= x; i++)
    {
        n = n * i % 998244353;
    }
    return n;
}
```
严重错误，即使进行了取模运算，但是取模运算是在相乘以后，这个时候编译器是以int来处理这个表达式，==这个极大值产生的瞬间，存储的值就会变成负数==，所以即使我把这个赋值的对象变成long long 也没用，必须一开始就将n的类型设置为long long ,或者把i的类型设置为long long