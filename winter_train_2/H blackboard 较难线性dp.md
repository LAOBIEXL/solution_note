![[Pasted image 20260220194823.png]]
![[Pasted image 20260220221050.png]]
![[Pasted image 20260220220919.png]]
![[Pasted image 20260220221000.png]]
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
const int M = 998244353;
#define endl '\n'

void solve()
{
    //不要忘记清空数组；
    //你个nt递归死路记得也要写返回；
    int n;
    cin >> n;
    vector<int> a(n + 1);
    vector<int> pre(n + 1); //标记对于第i位上一位最近的非零元素的下标；

    int lst = 0;  // lst是上一次非零传递下来的；

    for (int i = 1; i <= n;  i++)
    {
        cin >> a[i];
        pre[i] = lst; 
        if (a[i] > 0)
        {
            lst = i;
        }
    }
    vector<ll> s(n + 2);
    vector<ll> f(n + 2);


    //注意到： f[2] = f[1] + f[0] 
    //所以，对f的求和是从零开始的，借此，f[i]表示为i-1最合适，将f[0]变为f[1]便于前缀和；

    //f[1] = 1;
    ////f[0] = 1;
    //f[2] = 1;

    //s[0] = 0;
    //s[1] = 1;
    //s[2] = 2;

    //for (int i = 2; i <= n; i++)
    //{
    //    int j = i - 1;
    //    int val = a[i];
    //    while (j > 0 && (val & a[j]) == 0)
    //    {
    //        val = a[j] | val;
    //        j = pre[j]; //快速跨转非零位置，32次以内完成搜索；
    //    }
    //    f[i + 1] = ((s[i] - s[j])%M + M) % M;  //正常来说，前缀和肯定是越来越大，但是由于在计算前缀和的过程有取模，所以对于负数运算，一定要
    //    s[i + 1] = (s[i] + f[i + 1]) % M;
    //}

    f[1] = 1;
    s[0] = 0;
    s[1] = 1;


    for (int i = 1; i <= n; i++) // 不从原来的 i = 2 开始，可以兼容 n = 1的情况；
    {
        //首先计算f[2] 代表 f[1];
        int j = i;
        int val = 0;
        while (j > 0 && (val & a[j]) == 0)  // 注意这个位置优先级的问题，要加括号
        {
            val = val | a[j];
            j = pre[j];
        }
        f[i + 1] = ((s[i] - s[j]) % M + M) % M;
        s[i + 1] = (s[i] + f[i + 1]) % M;
    }

    cout << f[n + 1] % M << endl;
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