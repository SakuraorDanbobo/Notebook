### Background
Hugo Heavy is happy. After the breakdown of the Cargolifter project he can now expand business. But he needs a clever man who tells him whether there really is a way from the place his customer has build his giant steel crane to the place where it is needed on which all streets can carry the weight.  
Fortunately he already has a plan of the city with all streets and bridges and all the allowed weights.Unfortunately he has no idea how to find the the maximum weight capacity in order to tell his customer how heavy the crane may become. But you surely know.  
### Problem  
You are given the plan of the city, described by the streets (with weight limits) between the crossings, which are numbered from 1 to n. Your task is to find the maximum weight that can be transported from crossing 1 (Hugo's place) to crossing n (the customer's place). You may assume that there is at least one path. All streets can be travelled in both directions.
### Input
The first line contains the number of scenarios (city plans). For each city the number n of street crossings (1 <= n <= 1000) and number m of streets are given on the first line. The following m lines contain triples of integers specifying start and end crossing of the street and the maximum allowed weight, which is positive and not larger than 1000000. There will be at most one street between each pair of crossings.
### Output
The output for every scenario begins with a line containing "Scenario \#i:", where i is the number of the scenario starting at 1. Then print a single line containing the maximum allowed weight that Hugo can transport to the customer. Terminate the output for the scenario with a blank line.

### Input
```in
1
3 3
1 2 3
1 3 4
2 3 5
```
### Output
```in
Scenario #1:
4
```
### 题意
```in
给你n个点，m条边，每条边限制了最大运输重量k，求起点1到终点n的最大运输重量，即求1~n路径中最小运输重量的最大值。
```
### 思路
```in
最短路的求和改为了求极大值，即关系转移为dis[i]=max(dis[i],min(当前道路承重，这条路上的最小承重))。因为点的个数多于300个，因此无法使用Floyd，这里使用dijkstra。
```
### 参考代码
```c++
#include <iostream>
#include <algorithm>
#include <math.h>
#include <string.h>
#include <stdio.h>
#include <queue>
#include <map>
#include <set>
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
const int N = 1e3 + 5;
const int M = 2e5 + 5;
const int mod = 1e9 + 7;

int t, n, m;
vector<PII> edg[N];
int dis[N]; //dis[i]维护的是目前到达i点最大的代价.
bool vis[N];
void dijkstra()
{
	memset(dis, 0, sizeof dis);
	memset(vis, 0, sizeof vis);
	
	//<最小值,当前点>
	priority_queue<PII> heap; 
	heap.push({INF, 1});
	while (heap.size())
	{
		PII tmp = heap.top();
		heap.pop();
		int u=tmp.second;
		if (vis[u])
			continue;
		vis[u] = 1;
		for(int i=0;i<edg[u].size();i++){
			int nex=edg[u][i].first;
			int cost=edg[u][i].second;
			if(dis[nex]<tmp.first){
				dis[nex]=max(dis[nex],min(cost,tmp.first));
				heap.push({min(cost,tmp.first),nex});
			}
		}
	}
}
int main()
{
	io;
	cin >> t;
	for (int i = 1; i <= t; i++)
	{
		cin >> n >> m;
		
		for(int j=1;j<=n;j++)edg[j].clear();
		while (m--)
		{
			int a, b, c;
			cin >> a >> b >> c;
			edg[a].push_back({b, c});
			edg[b].push_back({a, c});
		}
		dijkstra();
		cout << "Scenario #" << i << ":\n";
		cout << dis[n] << "\n\n";
	}
	system("pause");
	return 0;
}
 
```