[[搜索]]
![[Pasted image 20251220221848.png]]

## 分析：
不要忘记出队，否则死循环的拉；

## 代码：
```cpp
#include <iostream>

#include <queue>

#include <cstring>

using namespace std;

typedef pair<int, int> PII;

const int N = 35;

int n, m, x, y;

char a[N][N];

int dist[N][N];

int dx[] = {0, 0, 1, -1};

int dy[] = {1, -1, 0, 0};

void bfs()

{

memset(dist, -1, sizeof dist);

queue<PII> q;

q.push({x, y});

dist[x][y] = 0;

while(q.size())

{

auto t = q.front(); q.pop();

int i = t.first, j = t.second;

for(int k = 0; k < 4; k++)

{

int x = i + dx[k], y = j + dy[k];

if(x >= 1 && x <= n && y >= 1 && y <= m && a[x][y] != '*' &&

dist[x][y] == -1)

{

dist[x][y] = dist[i][j] + 1;

if(a[x][y] == 'e')

{

continue;

}

q.push({x, y});

}

}

}

}

int main()

{

cin >> n >> m;

for(int i = 1; i <= n; i++)

{

for(int j = 1; j <= m; j++)

{

cin >> a[i][j];

if(a[i][j] == 'k')

{

x = i; y = j;

}

}

}

bfs();

// 统计结果

int cnt = 0, ret = 1e9;

for(int i = 1; i <= n; i++)

{

for(int j = 1; j <= m; j++)

{

if(a[i][j] == 'e' && dist[i][j] != -1)

{

cnt++;

ret = min(ret, dist[i][j]);

}

}

}

if(cnt == 0) cout << -1 << endl;

else cout << cnt << " " << ret << endl;

return 0;

}
```