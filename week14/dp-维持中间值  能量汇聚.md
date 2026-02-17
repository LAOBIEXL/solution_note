![[Pasted image 20251224183607.png]]
[[dp-线性DP]]

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
const int N = 1e5 + 10;
int f[2][N];
int a[N];


signed main()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        cin >> a[i];
    }
    f[0][1] = a[1];
    f[1][1] = 0; // 不可能；
    f[1][2] = a[2];
    f[0][2] = 0; // 不可能；
    for (int i = 3; i <= n; i++)
    {
        f[0][i] = f[1][i - 1] + a[i];
        f[1][i] = max(f[0][i - 2], f[1][i - 2]) + a[i];
    }
    int f_max = max(f[1][n],f[0][n]);
    cout << f_max << endl;
}
```