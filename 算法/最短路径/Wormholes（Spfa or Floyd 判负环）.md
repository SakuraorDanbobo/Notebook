#### 描述

While exploring his many farms, Farmer John has discovered a number of amazing wormholes. A wormhole is very peculiar because it is a one-way path that delivers you to its destination at a time that is BEFORE you entered the wormhole! Each of FJ's farms comprises N (1 ≤ N ≤ 500) fields conveniently numbered 1..N, M (1 ≤ M ≤ 2500) paths, and W (1 ≤ W ≤ 200) wormholes.

As FJ is an avid time-traveling fan, he wants to do the following: start at some field, travel through some paths and wormholes, and return to the starting field a time before his initial departure. Perhaps he will be able to meet himself :) .

To help FJ find out whether this is possible or not, he will supply you with complete maps to F (1 ≤ F ≤ 5) of his farms. No paths will take longer than 10,000 seconds to travel and no wormhole can bring FJ back in time by more than 10,000 seconds.

#### 输入

Line 1: A single integer, F. F farm descriptions follow.
Line 1 of each farm: Three space-separated integers respectively: N, M, and W
Lines 2..M+1 of each farm: Three space-separated numbers (S, E, T) that describe, respectively: a bidirectional path between S and E that requires T seconds to traverse. Two fields might be connected by more than one path.
Lines M+2..M+W+1 of each farm: Three space-separated numbers (S, E, T) that describe, respectively: A one way path from S to E that also moves the traveler back T seconds.

#### 输出

Lines 1..F: For each farm, output "YES" if FJ can achieve his goal, otherwise output "NO" (do not include the quotes).

#### 样例输入

2
3 3 1
1 2 2
1 3 4
2 3 1
3 1 3
3 2 1
1 2 3
2 3 4
3 1 8

#### 样例输出
NO
YES

#### 题意
```in
给你n个点，m条双向正权边，w条单向负权边，问是否存在负环。
```
#### 思路
```in
spfa 或者 floyd判断负权边。若要判断正权边，改为最远距离即可。
```
#### 参考代码
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 2e5 + 5;
const int M = 1e5;
const int mod = 1e9 + 7;

int t,n,m,l;
int ma[555][555];
/*
   题意：给你n个点，m条双向正权边，w条单向负权边，问是否存在负环。
   Spfa or Floyd
*/
bool floyd()
{
	for(int i=1;i<=n;i++)ma[i][i]=0;
	for(int k=1;k<=n;k++)
	{
		for(int i=1;i<=n;i++)
		{
			for(int j=1;j<=n;j++)
			{
				if(ma[i][j]>ma[i][k]+ma[k][j])
				{
					ma[i][j]=ma[i][k]+ma[k][j];
				}
			}
			if(ma[i][i]<0)return true;
		}
	}
	return false;
}

int cnt[555];
bool vis[555];
int dis[555];
bool spfa()
{
	memset(dis,0x3f,sizeof dis);
	memset(cnt,0,sizeof cnt);
	queue<int>heap;
	for(int i=1;i<=n;i++)
	{
		heap.push(i);
		vis[i]=1;
	}
	while(heap.size())
	{
		int u=heap.front();
		heap.pop();
		vis[u]=0; 

		for(int i=1;i<=n;i++)
		{
			if(ma[u][i]!=INF)
			{
				if(dis[i]>dis[u]+ma[u][i])
				{
					dis[i]=dis[u]+ma[u][i];
					cnt[i]=cnt[u]+1;
					if(!vis[i])
					{
						heap.push(i);
						vis[i]=1;
					}
					if(cnt[i]>=n)return true;
				}
			}
		}
	}
	 return false;
}
int main()
{
	io;
	
	int x,y,z;
	cin>>t;
	while(t--)
	{
		cin>>n>>m>>l;
		memset(ma,0x3f,sizeof ma);
		while(m--)
		{
			cin>>x>>y>>z;
			if(ma[x][y]>z)ma[x][y]=ma[y][x]=z;
		}
		while(l--)
		{
			cin>>x>>y>>z;
			z*=-1;
			if(ma[x][y]>z)ma[x][y]=z;
		}
 
		if(spfa())cout<<"YES\n";
		else cout<<"NO\n";

	}
	


	// system("pause");
	return 0;
}
/*
 
*/

```