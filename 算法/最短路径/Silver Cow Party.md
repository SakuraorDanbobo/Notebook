描述

One cow from each of N farms (1 ≤ N ≤ 1000) conveniently numbered 1..N is going to attend the big cow party to be held at farm #X (1 ≤ X ≤ N). A total of M (1 ≤ M ≤ 100,000) unidirectional (one-way roads connects pairs of farms; road i requires Ti (1 ≤ Ti ≤ 100) units of time to traverse.

Each cow must walk to the party and, when the party is over, return to her farm. Each cow is lazy and thus picks an optimal route with the shortest time. A cow's return route might be different from her original route to the party since roads are one-way.

Of all the cows, what is the longest amount of time a cow must spend walking to the party and back?

输入

Line 1: Three space-separated integers, respectively: N, M, and X
Lines 2..M+1: Line i+1 describes road i with three space-separated integers: Ai, Bi, and Ti. The described road runs from farm Ai to farm Bi, requiring Ti time units to traverse.

输出

Line 1: One integer: the maximum of time any one cow must walk.

样例输入

4 8 2
1 2 4
1 3 2
1 4 7
2 1 1
2 3 5
3 1 2
3 4 4
4 2 3

样例输出

10

提示

Cow 4 proceeds directly to the party (3 units) and returns via farms 1 and 3 (7 units), for a total of 10 time units.
 



题意：给你n个点，m条单向路径，问 （第 i 个点去x点的路径 + x点去i点的路径 ）的最大值

思路：跑个最短路即可。以下使用 堆优化的*Dijkstra*。

参考代码：

```c++
#include <iostream>
#include <algorithm>
#include <string.h>
#include <queue>
#define LL long long
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define x first
#define y second
#define PII pair<int,int>
#define PDD pair<double,double>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e4+5;
const int M=1e7;

int n,m,x;
vector<int>ma[1002]; //邻接表存边 
int cost[1002][1002];
int ans[1002];
int dis[1002];

//最小堆优化迪杰斯特拉 
void dijstra(int u)
{
	//堆按照first从小到大排序 
	priority_queue<PII,vector<PII>,greater<PII> > p;
	memset(dis,INF,sizeof dis);
	dis[u]=0;
	p.push({0,u});
	while(p.size())
	{
		PII g=p.top();
		p.pop();
		if(g.x>dis[g.y])continue;
		for(int i=0;i<ma[g.y].size();i++)
		{
			int v=ma[g.y][i];
			if(dis[v]>dis[g.y]+cost[g.y][v])
			{
				dis[v]=dis[g.y]+cost[g.y][v];
				p.push({dis[v],v});
			}
		}
	}
	//u号牛去的路 
	if(u!=x)
	{
		ans[u]+=dis[x];
	}
	else
	{
		//i号点回去的路 
		for(int i=1;i<=n;i++)
		{
			if(i!=x)
			{
				ans[i]+=dis[i];
			}
		}
	}
}
int main()
{
	io;
	cin>>n>>m>>x;
	memset(cost,INF,sizeof cost);
	for(int i=1,a,b,c;i<=m;i++)
	{
		cin>>a>>b>>c;
		ma[a].push_back(b);
		cost[a][b]=c;
	}
	
	for(int i=1;i<=n;i++)
	{
		dijstra(i);
	}	
	
	int res=-1;
	for(int i=1;i<=n;i++)
	{
		res=max(res,ans[i]);
	}
	cout<<res<<"\n";
}

```

