[[dp-动态规划入门]]
![[Pasted image 20251206095657.png]]

## 分析：
对于第n个决策，最大值可以由最小值反转产生，也可以由最大值累加产生，所以，只需要用两个动态规划保存最大值到最小 值的范围，就可以判断留存到最后的最小值是什么；

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
const int N = 1e5 + 10;
ll a[N], b[N];
ll f1[N], f2[N];

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int n;
        cin >> n;
        ll k = 0;
        for (int i = 1; i <= n; i++) cin >> a[i];
        for (int i = 1; i <= n; i++) cin >> b[i];
        for (int i = 1; i <= n; i++)
        {
            f1[i] = max(f1[i - 1] - a[i], b[i] - f2[i - 1]);
            f2[i] = min(f2[i - 1] - a[i], b[i] - f1[i - 1]);
        }
        cout << f1[n] << endl;
    }


    return 0;
}

```
