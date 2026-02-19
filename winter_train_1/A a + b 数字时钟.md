[[位运算和状态压缩]]
[[快速幂]]
![[Pasted image 20260217205733.png]]

每一个灯管都是独立的，利用状态压缩表示每一个灯管的开关，计算出每个数字的概率，然后枚举每一种情况求和；

求 p %  - 》 （1 - p ) % 
在取模运算下，两个概率相加为一仍然成立，但是
![[Pasted image 20260217210003.png]]


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
const ll N = 998244353;
#define endl '\n'
vector<ll> nums(10);


ll qpow(ll a, ll m)
{
    ll t = a;
    ll ret = 1;
    while (m)
    {
        if (m & 1) ret = ret * t % N;
        t = t * t % N;
        m >>= 1;
    }
    return ret;
}

ll calc(ll i,vector<ll> &p,vector<ll> &p2)
{
    ll ret = 1;
    ll cnt = 0;
    ll t = nums[i];
    while (cnt <= 6)
    {
        if (t & 1) {
            ret = ret * p[cnt] % N;
        }
        else
        {
            ret = ret * p2[cnt] % N;
        }
        cnt++;
        t >>= 1;
    }
    return ret;
}

ll calnum(ll cur, vector<ll> & p1)
{
    ll ans = 0;
    ll t1 = cur % 10;
    ll t2 = (cur / 10) % 10;
    ll t3 = (cur / 100) % 10;
    ll t4 = (cur / 1000) % 10;
    ans = ((p1[t1] * p1[t2]) % N) * (p1[t3] * p1[t4] % N) % N;
    return ans;

}

void solve()
{
    //不要忘记清空数组；
    ll C;
    cin >> C;
    vector<ll> p(8);
    vector<ll> p2(8);
    for (int i = 0; i <= 6; i++)
    {
        cin >> p[i]; 
        p2[i] = (100 - p[i]) * qpow(100, N - 2) % N;
        p[i] = p[i] * qpow(100, N - 2) % N;
    }
    vector<ll> p1(10);

    for (int i = 0; i <= 9; i++)
    {
        p1[i] = calc(i, p, p2);      
    }

    ll cur = 0;
    ll ans = 0;
    while (cur <= C)
    {
        ans = (ans + calnum(cur, p1) * calnum(C - cur, p1) % N) % N;
        cur++;
    }

    cout << ans % N << endl;

}

signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    nums[0] = 1 << 0 | 1 << 1 | 1 << 2 | 1 << 4 | 1 << 5 | 1 << 6;
    nums[1] = 1 << 2 | 1 << 5;
    nums[2] = 1 << 0 | 1 << 2 | 1 << 3 | 1 << 4 | 1 << 6;
    nums[3] = 1 << 0 | 1 << 2 | 1 << 3 | 1 << 5 | 1 << 6;
    nums[4] = 1 << 1 | 1 << 2 | 1 << 3 | 1 << 5;
    nums[5] = 1 << 0 | 1 << 1 | 1 << 3 | 1 << 5 | 1 << 6;
    nums[6] = 1 << 0 | 1 << 1 | 1 << 3 | 1 << 4 | 1 << 5 | 1 << 6;
    nums[7] = 1 << 0 | 1 << 2 | 1 << 5;
    nums[8] = 1 << 0 | 1 << 1 | 1 << 2 | 1 << 3 | 1 << 4 | 1 << 5 | 1 << 6;
    nums[9] = 1 << 0 | 1 << 1 | 1 << 2 | 1 << 3 | 1 << 5 | 1 << 6;

    int TestCase = 1;
    cin >> TestCase;
    while (TestCase--)
        solve();
}
```