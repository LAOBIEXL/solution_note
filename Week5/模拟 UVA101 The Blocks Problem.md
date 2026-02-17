# UVA101 The Blocks Problem

## 题目描述

[problemUrl]: https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=3&page=show_problem&problem=37

[PDF](https://uva.onlinejudge.org/external/1/p101.pdf)

![](https://cdn.luogu.com.cn/upload/vjudge_pic/UVA101/4657c698576c8c299dbbf5655d7dbe63bf148978.png)

## 输入格式

![](https://cdn.luogu.com.cn/upload/vjudge_pic/UVA101/0a0a9b4a15235d9e81d83d5d31ee89ce48870fed.png)

## 输出格式

![](https://cdn.luogu.com.cn/upload/vjudge_pic/UVA101/ca24bcd0ff3af9dc6c1fcefd73c87532e9e05bd4.png)

## 输入输出样例 #1

### 输入 #1

```
10
move 9 onto 1
move 8 over 1
move 7 over 1
move 6 over 1
pile 8 over 6
pile 8 over 5
move 2 over 1
move 4 over 9
quit
```

### 输出 #1

```
0: 0
1: 1 9 2 4
2:
3: 3
4:
5: 5 8 7 6
6:
7:
8:
9:
```


# 题解
```cpp
#include<vector>
#include<iostream>
using namespace std;

const int N = 30;
vector<int> blocks[N];
int n;

int a1, a2, a3, a4;

void clear(int x)
{
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < blocks[i].size(); j++)
		{
			if (blocks[i][j] == x)
			{
				for (int t = j + 1; t < blocks[i].size(); t++)
				{
					blocks[blocks[i][t]].push_back(blocks[i][t]);
				}
				blocks[i].resize(j + 1);
			}
		}
	}
}

void find(int a, int b)
{
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < blocks[i].size(); j++)
		{
			if (blocks[i][j] == b)
			{
				a1 = i;
				a2 = j;
			}
		}
	}
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < blocks[i].size(); j++)
		{
			if (blocks[i][j] == a)
			{
				a3 = i;
				a4 = j;
			}
		}
	}
}

void put(int a, int b)
{
	for (int i = a2; i < blocks[a1].size(); i++)
	{
		blocks[a3].push_back(blocks[a1][i]);
	}
	blocks[a1].resize(a2);
}

int main()
{
	//存储初始化
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		blocks[i].push_back(i);
	}
	string a, b;
	//读取操作与分类：
	while (1)
	{
		
		int n1, n2;
		cin >> a;
		if (a == "quit")
		{
			break;
		}

		cin  >> n1 >> b >> n2;
		
		find(n2,n1);
		
		if (a1 == a3)
		{
			continue;
		}
		
		if (a == "move")
		{
			if (b == "onto")
			{
				//设定操作 ： 清空某个节点 clear
				//设定操作，将某个节点及以上移动到另一个栈顶：put()
				clear(n1);
				clear(n2);
				put(n2, n1);
			}
			else
			{
				clear(n1);
				put(n2, n1);
			}
		}
		else
		{
			if (b == "onto")
			{
				clear(n2);
				put(n2, n1);
			}
			else
			{
				put(n2, n1);
			}
		}

	}
	
	//输出：
	for (int i = 0; i < n; i++)
	{
		cout << i << ':' << ' ';
		for (int j = 0; j < blocks[i].size(); j++)
		{
			cout << blocks[i][j] << " ";
		}
		cout << endl;
	}


	return 0;
}

```

经验总结：
对于读取字符串来结束，需要将特殊的判断字符串单独写一个cin，这样可以单独读取和判断；

模拟操作要尽可能的归纳提炼，以形成更少的类型和更多的共性；
