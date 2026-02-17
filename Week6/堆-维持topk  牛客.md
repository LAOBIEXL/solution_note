![[Pasted image 20251102131835.png]]

### 思路：
维持第k小的核心就是每次有新元素进入，都进行大小排序，在最小的k个中最大的那一个就是第k小，核心就是矮子里面挑将军，所以第k小用的是大根堆，第k大用的是小根堆；

### 题解：
```cpp

#include<iostream>
#include<queue>
using namespace std;

priority_queue<int> heap;

int main()
{
	int n, m, k;
	cin >> n >> m >> k;
	//初始化堆；

	for (int i = 1; i <= n; i++)
	{
		int t;
		cin >> t;
		heap.push(t);
		if (heap.size() > k) heap.pop();
	}

	//读取操作；
	while (m--)
	{
		int op = 0;
		cin >> op;
		if (op == 1)
		{
			int d;
			cin >> d;
			heap.push(d);
			if (heap.size() > k)
			{
				heap.pop();
			}
		}
		else if(op == 2)
		{
			if (heap.size() < k)
				cout << -1 << endl;
			else
				cout << heap.top() << endl;
		}
	}

}
```