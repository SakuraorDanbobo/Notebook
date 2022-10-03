### Question:
In graph theory, an Eulerian path is a path in a graph which visits every edge exactly once. Similarly, an Eulerian circuit is an Eulerian path which starts and ends on the same vertex. They were first discussed by Leonhard Euler while solving the famous Seven Bridges of Konigsberg problem in 1736. It has been proven that connected graphs with all vertices of even degree have an Eulerian circuit, and such graphs are called **Eulerian**. If there are exactly two vertices of odd degree, all Eulerian paths start at one of them and end at the other. A graph that has an Eulerian path but not an Eulerian circuit is called **semi-Eulerian**. (Cited from https://en.wikipedia.org/wiki/Eulerian_path)

Given an undirected graph, you are supposed to tell if it is Eulerian, semi-Eulerian, or non-Eulerian.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 2 numbers N (≤ 500), and M, which are the total number of vertices, and the number of edges, respectively. Then M lines follow, each describes an edge by giving the two ends of the edge (the vertices are numbered from 1 to N).

### Output Specification:

For each test case, first print in a line the degrees of the vertices in ascending order of their indices. Then in the next line print your conclusion about the graph -- either `Eulerian`, `Semi-Eulerian`, or `Non-Eulerian`. Note that all the numbers in the first line must be separated by exactly 1 space, and there must be no extra space at the beginning or the end of the line.

### Sample Input 1:

```in
7 12
5 7
1 2
1 3
2 3
2 4
3 4
5 2
7 6
6 3
4 5
6 4
5 6
```

### Sample Output 1:

```out
2 4 4 4 4 4 2
Eulerian
```

### Sample Input 2:

```in
6 10
1 2
1 3
2 3
2 4
3 4
5 2
6 3
4 5
6 4
5 6
```

### Sample Output 2:

```out
2 4 4 4 3 3
Semi-Eulerian
```

### Sample Input 3:

```in
5 8
1 2
2 5
5 4
4 1
1 3
3 2
3 4
5 3
```

### Sample Output 3:

```out
3 3 4 3 3
Non-Eulerian
```

### 题意：
```in
给你n个点，m条边，问判断是以下哪3种情况？
1.欧拉图，Eulerian
2.欧拉回路，Semi-Eulerian
3.2者都不是，Non-Eulerian
```
### 思路：
```in
1.判断欧拉的前提是该图连通，因此可以用并查集，DFS等判图连通。
2.接着就是统计奇度点和偶度点的个数了，
	a.所有点都是偶度点，则它就是Eulerian，
	b.如果除了两个结点的度是奇数其他都是偶数，那么它就是Semi-Eulerian，
	c.否则就是Non-Eulerian。
```
### 参考代码：
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PLL pair<LL, LL>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1e5 + 5;
const int M = 2e5+5;
const int mod = 1e9 + 7;
 
int d[666];
int fa[666];
int find(int x){return fa[x]==x?x:fa[x]=find(fa[x]);}
void join(int a,int b){fa[find(b)]=find(a);}
int main()
{
	// io;
	int n,m,k,s;
	cin>>n>>m;
	for(int i=1;i<=n;i++)fa[i]=i;
	for(int i=1;i<=m;i++)
	{
		int a,b;
		cin>>a>>b;
		join(a,b);
		d[a]++;
		d[b]++;
	}
	int odd=0,even=0;
	int num=0;
	for(int i=1;i<=n;i++)
	{
		if(i!=1)cout<<" ";
		cout<<d[i];
		if(d[i]&1)odd++;
		else even++;
		if(i==fa[i])num++;
	}
	if(num!=1)cout<<"\nNon-Eulerian";
	else if(even==n)cout<<"\nEulerian";
	else if(odd==2)cout<<"\nSemi-Eulerian";
	else cout<<"\nNon-Eulerian";
	// system("pause");
	return 0;
}
/*
5
2 2 3
2 1 5
1 4
1 3
0
111
*/


```