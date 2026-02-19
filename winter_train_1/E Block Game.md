快速数组循环索引：
[[小技巧]]
```cpp
void solve()
{
    //不要忘记清空数组；
    int n, k;
    cin >> n >> k;

    n++;

    vector<int> a(n, k);
    for (int i = 0; i < n - 1; i++)
    {
        cin >> a[i];
    }
    int ans = -0x3f3f3f3f;
    for (int i = 0; i < n; i++)
    {
        ans = max(ans, a[i] + a[(i + 1) % n]);
    }

    cout << ans << endl;
}
```
循环寻找最大的相邻元素之和：
```cpp
 for (int i = 0; i < n; i++)
    {
        ans = max(ans, a[i] + a[(i + 1) % n]);
    }

```
