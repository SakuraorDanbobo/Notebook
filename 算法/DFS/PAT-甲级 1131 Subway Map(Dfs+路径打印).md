### Question:
In the big cities, the subway systems always look so complex to the visitors. To give you some sense, the following figure shows the map of Beijing subway. Now you are supposed to help people with your computer skills! Given the starting position of your user, your task is to find the quickest way to his/her destination.
### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer N (≤ 100), the number of subway lines. Then N lines follow, with the i-th (i=1,⋯,N) line describes the i-th subway line in the format:

M S[1] S[2] ... S[M]

where M (≤ 100) is the number of stops, and S[i]'s (i=1,⋯,M) are the indices of the stations (the indices are 4-digit numbers from 0000 to 9999) along the line. It is guaranteed that the stations are given in the correct order -- that is, the train travels between S[i] and S[i+1] (i=1,⋯,M−1) without any stop.

Note: It is possible to have loops, but not self-loop (no train starts from S and stops at S without passing through another station). Each station interval belongs to a unique subway line. Although the lines may cross each other at some stations (so called "transfer stations"), no station can be the conjunction of more than 5 lines.

After the description of the subway, another positive integer K (≤ 10) is given. Then K lines follow, each gives a query from your user: the two indices as the starting station and the destination, respectively.

The following figure shows the sample map.
![[Pasted image 20220531190300.png]]

Note: It is guaranteed that all the stations are reachable, and all the queries consist of legal station numbers.

### Output Specification:

For each query, first print in a line the minimum number of stops. Then you are supposed to show the optimal path in a friendly format as the following:

```
Take Line#X1 from S1 to S2.
Take Line#X2 from S2 to S3.
......
```

where `X` i's are the line numbers and  `S` i's are the station indices. Note: Besides the starting and ending stations, only the transfer stations shall be printed.

If the quickest path is not unique, output the one with the minimum number of transfers, which is guaranteed to be unique.

### Sample Input:

```in
4
7 1001 3212 1003 1204 1005 1306 7797
9 9988 2333 1204 2006 2005 2004 2003 2302 2001
13 3011 3812 3013 3001 1306 3003 2333 3066 3212 3008 2302 3010 3011
4 6666 8432 4011 1306
3
3011 3013
6666 2001
2004 3001
```

### Sample Output:

```out
2
Take Line#3 from 3011 to 3013.
10
Take Line#4 from 6666 to 1306.
Take Line#3 from 1306 to 2302.
Take Line#2 from 2302 to 2001.
6
Take Line#2 from 2004 to 1204.
Take Line#1 from 1204 to 1306.
Take Line#3 from 1306 to 3001.
```

### 题意:
```in
给定n条地铁线路，k次询问，找出一条路线，使得对任何给定的起点和终点，可以找出中途经停站最少的路线；如果经停站一样多，则取需要换乘线路次数最少的路线。
```
### 思路:
```in
首先，对于2个站点属于哪条线路，我们可以进行如下操作：
若第一个站点id为a，第二个站点id为b，则我们使用map去记录，a*10000+b与b*10000+a（因为是无无向图，所以a->b和b->a都要记录）。

其次，跑一遍DFS，同时使用tmppath记录路径上的站点。DFS过程中要维护两个变量：minCnt-中途经停的最少的站; minTrans-需要换乘的最小次数。

最后，打印路径即可。
```
### 参考代码：
```c++
#include <bits/stdc++.h>
using namespace std;
const int N=1e4+5;
const int INF=0x3f3f3f3f;
//判断2个站点属于哪条线路，用a*10000+b来当key 
unordered_map<int,int> line;  
//邻接表存边 
vector<int>ma[N]; 
//最少站数,最少转站数 
int mincnt,mintrans;
//判断点是否访问
int vis[N]; 
//路径存储
vector<int>path,tmppath; 

//得到路径上的转站数 
int findtrans()
{
	int len=tmppath.size(); 
	int pline=-1;
	int res=0;
	for(int i=1,l,r,cline;i<len;i++)
	{
		l=tmppath[i-1];
		r=tmppath[i];
		cline=line[l*10000+r];
		if(cline!=pline)res++;
		pline=cline;
	}
	return res;
} 
//dfs暴搜找路径 
void dfs(int cur,int step,int ed)
{
	if(cur==ed)
	{
		int num=findtrans();
		if(mincnt>step)
		{
			path=tmppath;
			mincnt=step;
			mintrans=num;
		}
		else if(mincnt==step && mintrans>num)
		{
			path=tmppath;
			mintrans=num;
		}
		return;	
	} 
	for(auto nex:ma[cur])
	{
		if(vis[nex])continue;
		vis[nex]=1;
		tmppath.push_back(nex);
		dfs(nex,step+1,ed);
		tmppath.pop_back();
		vis[nex]=0;	
	}
}
int main()
{
    int n,m;
	cin>>n;
	//建边 
	for(int i=1,pre,x;i<=n;i++)
	{
		cin>>m;
		cin>>pre;
		for(int j=2;j<=m;j++)
		{
			cin>>x;
			//确定线路
			line[x*10000+pre]=line[pre*10000+x]=i; 
			//存边 
			ma[pre].push_back(x);
			ma[x].push_back(pre);
			pre=x;
		}
	}
	
	int k,sta,end;
	cin>>k;
	while(k--)
	{
		cin>>sta>>end;
		
		mincnt=mintrans=INF; 
		tmppath.clear();
		vis[sta]=1;
		tmppath.push_back(sta);
		dfs(sta,0,end);
		vis[sta]=0;
		cout<<mincnt<<"\n";
		int pline=0,pretrans=sta;
		for(int i=1,l,r,cline;i<path.size();i++)
		{
			l=path[i-1];
			r=path[i];
			cline=line[l*10000+r];
			if(cline!=pline)
			{
				if(pline!=0)printf("Take Line#%d from %04d to %04d.\n",pline,pretrans,l);
				pline=cline;
				pretrans=l;
			}
		}
		printf("Take Line#%d from %04d to %04d.\n",pline,pretrans,end);
	
	}
}
```