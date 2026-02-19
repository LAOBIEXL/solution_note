[[贪心]]
[[数位dp]]

![[Pasted image 20260218111751.png]]

1. 数位越多，数字越大；
2. 越低的位置上放越多的9，数字越大；
3. 在翻转前，只需要在最高位上退1， 就可以使后面的位置上全部都是9；
4. 特殊情况：
- r 与 r 相等；
- r 最高位退1 的结果还没有直接翻转来的大；

### 处理方式：
1. 数学整除运算：涉及数位运算，高次的幂次：
![[Pasted image 20260218112034.png]]

2. 推荐使用字符串：
```cpp
stoi()  stoll() : string -> long long 
```

3. trick : 在数字阶段判断分歧点：
```cpp
int l , r;
cin >> l >> r;
int p = 1;
while(l / p != r / p) p *= 10;
表示两个数在p数量级任然是相同的

那么不同的数量级就是 ：
p /= 10;

```
[[小技巧]]

## 贪心解法：
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

ll get_fold(ll x)
{
    string t = to_string(x);
    reverse(t.begin(), t.end());
    
    return stoll(t);
}

void solve()
{
    //不要忘记清空数组；
    string L, R;
    cin >> L >> R;
    ll l = stoll(L), r = stoll(R);
    ll maxbase = max(get_fold(l), get_fold(r));
    ll lenr = R.size(), lenl = L.size();
    if (lenr > lenl)
    {
        int ok = 1;
        int i = 1;
        while (i < lenr)
        {
            ok &= (R[i] == '0');
            if (!ok) break;
            i++;
        }
        if (ok && R[0] == '1')
        {
            cout << r - 1 << endl;
        }
        else
        {
            if (R[0] != '1')
            {
                i = 0;
            }
            R[i++] -= 1;
            for (int j = i; j < lenr; j++)
            {
                R[j] = '9';
            }
            ll cmp = get_fold(stoll(R));
            if (cmp > maxbase)
            {
                cout << cmp << endl;
            }
            else
            {
                cout << maxbase << endl;
     
            }
         
        }
    }
    else
    {
        int pos = 0;
        while (L[pos] == R[pos] && pos < lenr)
        {
            pos++;
        }
        if (pos == lenr )
        {
            cout << maxbase << endl;

        }
        else
        {
            R[pos++] -= 1;
            while (pos < lenr)
            {
                R[pos] = '9';
                pos++;
            }
            ll cmp = get_fold(stoll(R));
            if (cmp > maxbase)
            {
                cout << cmp << endl;
            }
            else
            {
                cout << maxbase << endl;
            }
        }
    }
 

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

## 总结：
[[错误汇总]]
1. 在修改判断条件的时候要有清晰的分支意识，尤其是出现错误之后加入的判断，可能加错位置或者导致整个循环结构出现问题；
2. 在判断相等时，要同时加入两个条件，不然一定会越界访问：![[Pasted image 20260218201652.png]]


## 数位dp版：


