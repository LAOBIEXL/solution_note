[[搜索]][[预处理]]
![[Pasted image 20251224194135.png]]

## 分析：
之前一直好奇如果是全排列或者组合数类型的，或者是数独类型的一个空一个空填的题目如何写bfs形式，通常情况下，这种类型每次填一个空，然后向下纵深，但是bfs就是把第一次能做的改变全做了，然后判断合理性，如果合理，就加入到队列中，这样第一次出现结果时进行的操作次数就是所求的答案；

另外由于需要进行素数判断，所以最好在题目前面先进行完成素数的预处理：
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
const int N = 10000;
#define endl '\n'
bool nums[N];
int dist[N];

void solve()
{
    memset(dist, -1, sizeof(dist));
    //不要忘记清空数组；
    int a, b;
    cin >> a >> b;
    queue<int> q;
    q.push(a);
    int flag = 1;
    dist[a] = 0;
    if(a == b)
    {
        cout << 0 << endl;
        return ;
    }
    while (q.size())
    {
        auto t = q.front();
        q.pop();
       
        for (int k = 0; k < 4; k++)
        {
            if (flag == 0) break;
            for (int i = 0; i <= 9; i++)
            {
                int x = 0;
                if (k == 0)
                {
                    x = t / 10;
                    x = x * 10 + i;
                  
                }
                else if (k == 1)
                {
                    int tmp = t % 10;
                    x = t / 100;
                    x = x * 100 + tmp + 10 * i;
                }
                else if (k == 2)
                {
                    int tmp = t % 100;
                    x = t / 1000;
                    x = x * 1000 + tmp + 100 * i;
                }
                else
                {
                    x = t % 1000;
                    x = x + 1000 * i;
                }
                if (nums[x] && dist[x] == -1)
                {
                    dist[x] = dist[t] + 1;
                    q.push(x);
                    if (x == b)
                    {
                        cout << dist[x] << endl;
                        flag = 0;
                        break;
                    }
                }

            }
        }

    }
    if (flag) cout << -1;

}

signed main()
{
    for (int i = 2; i <= N; i++)
    {
        int flag = 1;
        for (int j = 2; j <= sqrt(i); j++)
        {
            if (i % j == 0)
            {
                flag = 0;
                break;
            }
        }
        if (flag) nums[i] = true;
    }



    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int TestCase = 1;
    cin >> TestCase;
    while (TestCase--)
        solve();
}
```