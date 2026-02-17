# UVA11572 唯一的雪花 Unique Snowflakes
[[滑动窗口：]]
## 题目描述

企业家 Emily 有一个很酷的主意：把雪花包起来卖。她发明了一台机器，这台机器可以捕捉飘落的雪花，并把它们一片一片打包进一个包裹里。一旦这个包裹满了，它就会被封上送去发售。

Emily 的公司的口号是“把独特打包起来”，为了实现这一诺言，一个包裹里不能有两片一样的雪花。不幸的是，这并不容易做到，因为实际上通过机器的雪花中有很多是相同的。Emily 想知道这样一个不包含两片一样的雪花的包裹最大能有多大，她可以在任何时候启动机器，但是一旦机器启动了，直到包裹被封上为止，所有通过机器的雪花都必须被打包进这个包裹里，当然，包裹可以在任何时候被封上。

## 输入格式

第一行是测试数据组数 $T$，对于每一组数据，第一行是通过机器的雪花总数 $n$（$n \le {10}^6$），下面 $n$ 行每行一个在 $[0, {10}^9]$ 内的整数，标记了这片雪花，当两片雪花标记相同时，这两片雪花是一样的。

## 输出格式

对于每一组数据，输出最大包裹的大小。

## 输入输出样例 #1

### 输入 #1

```
1
5
1
2
3
2
1
```

### 输出 #1

```
3
```


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
const int N = 1e6;
int arr[N];

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++) cin >> arr[i];
        int left = 1; int right = 1;
        unordered_map<int, int > mp;
        int ret = 0;
        while (right <= n)
        {
            mp[arr[right]]++;
            while (mp[arr[right]] > 1)
            {
                mp[arr[left]]--;
                left++;
            }
            ret = max(ret, right - left + 1);
            right++;
        }
        cout << ret << endl;
    }


    return 0;
}
```

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
const int N = 1e6;
int arr[N];

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++) cin >> arr[i];
        int left = 1; int right = 1;
        unordered_set<int > mp;
        int ret = 0;
        while (right <= n)
        {
            while (mp.count(arr[right]))
            {
                mp.erase(arr[left]);
                left++;
            }
            mp.insert(arr[right]);
            ret = max(ret, right - left + 1);
            right++;//这个一定不要忘；
            
        }
        cout << ret << endl;
    }


    return 0;
}

```
![[Pasted image 20251203215014.png]]
