## Rank of  Tetris ##

>​	对于=号的处理，一见到还以为没啥用，以为虚晃一下，直接拓扑板子冲就完事了，结果属实是没理解题意了。后来看了下题解，并查集解决等号（相同排名），剩下的就是裸的拓扑排序，属实还得多写多想多做哎。

**Problem Description**

自从Lele开发了Rating系统，他的Tetris事业更是如虎添翼，不久他遍把这个游戏推向了全球。

为了更好的符合那些爱好者的喜好，Lele又想了一个新点子：他将制作一个全球Tetris高手排行榜，定时更新，名堂要比福布斯富豪榜还响。关于如何排名，这个不用说都知道是根据Rating从高到低来排，如果两个人具有相同的Rating，那就按这几个人的RP从高到低来排。

终于，Lele要开始行动了，对N个人进行排名。为了方便起见，每个人都已经被编号，分别从0到N-1,并且编号越大，RP就越高。
同时Lele从狗仔队里取得一些（M个）关于Rating的信息。这些信息可能有三种情况，分别是"A > B","A = B","A < B"，分别表示A的Rating高于B,等于B,小于B。

现在Lele并不是让你来帮他制作这个高手榜，他只是想知道，根据这些信息是否能够确定出这个高手榜，是的话就输出"OK"。否则就请你判断出错的原因，到底是因为信息不完全（输出"UNCERTAIN"），还是因为这些信息中包含冲突（输出"CONFLICT"）。
注意，如果信息中同时包含冲突且信息不完全，就输出"CONFLICT"。

**Input**

本题目包含多组测试，请处理到文件结束。
每组测试第一行包含两个整数N,M(0<=N<=10000,0<=M<=20000),分别表示要排名的人数以及得到的关系数。
接下来有M行，分别表示这些关系

**Output**

对于每组测试，在一行里按题目要求输出

**Sample Input**

3 3
0 > 1
1 < 2
0 > 2
4 4
1 = 2
1 > 3
2 > 0
0 > 1
3 3
1 > 0
1 > 2
2 < 1

**Sample Output**

OK
CONFLICT
UNCERTAIN

思路：

并查集：用于无向的边(即 = 情况，说明相等的点是等价的)，可以用1个点来代替相等点的集合，也就是缩点。
拓扑：1个个点去掉，如果同时可以去掉2个点或以上的点，则说明信息不全(存在确定的先后关系)，如果判断内还有点不能去掉，则说明是有环，即冲突。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N=1e4+5;
int n,m,ans,l,r;
int a[N<<1],b[N<<1];
char c[N<<1];

int pre[N]; //父节点，判断2人排名是否相同 
int d[N]; //入度
vector<int> v[N]; //邻接表存边 
/*
并查集：并查集：用于无向的边(即 = 情况，说明相等的点是等价的)，可以用1个点来代替相等点的集合，也就是缩点。
拓扑：1个个点去掉，如果同时可以去掉2个点或以上的点，则说明信息不全(存在确定的先后关系)，
如果这之后判断内还有点不能去掉，则说明是有环，即冲突。
*/ 

//初始化 
void init()
{
	for(int i=0;i<n;i++)
	{
		v[i].clear();
		pre[i]=i;
		d[i]=0; 
	}			
}
//找父节点 
int find(int x)
{
	if(x!=pre[x])pre[x]=find(pre[x]);
	return pre[x];
}
//拓扑排序 
void toposolve()
{
	queue<int> q; //队列中存缩完点的序号 
	//是否确定 
	bool only=false; 
	for(int i=0;i<n;i++)
	{
		//根节点为自己(缩点的父节点)，并且入度为0，入队列 
		int l=pre[i]; 
		if(l==i && d[l]==0)
		{
			q.push(pre[i]); 
		}
	}
	while(!q.empty())
	{
		//当前处理点>1，说明处理点之间没有明确排名关系 
		//不能退出当前循环，因为还可能存在环 
		if(q.size()>1)only=true;	
		int next=q.front();
		q.pop();
		//人数-1 
		ans--;
		for(int i=0;i<v[next].size();i++)
		{
			d[v[next][i]]--;
			if(d[v[next][i]]==0)q.push(v[next][i]);
		 } 
	} 
	//说明存在环,冲突 
	if(ans>0)puts("CONFLICT");
	//信息不全 
	else if(only)puts("UNCERTAIN");
	else puts("OK");
}
int main()
{
	while(~scanf("%d%d",&n,&m))
	{
		//初始化 
		init();
		//排名总人数 
		ans=n;
		for(int i=0;i<m;i++)
		{
			scanf("%d %c %d",&a[i],&c[i],&b[i]);
			//相等情况，看成一个点判别,即缩点 
			if(c[i]=='=')
			{
				//并查集处理 = 
				l=find(a[i]);
				r=find(b[i]);
				if(l!=r)
				{
					//合并 
					pre[r]=l; 
					//人数-1 
					ans--;
				}
			}
		}
		int tmp,g,h; 
		for(int i=0;i<m;i++)
		{
			//处理 > 和 < 的情况 
			if(c[i]=='=')continue;
			g=find(a[i]);
			h=find(b[i]);
			//保证a>b情形 
			if(c[i]=='<')
			{
				tmp=g;
				g=h;
				h=tmp;
			}
			//入度++ 
			d[h]++;
			//存储邻边关系 
			v[g].push_back(h);
		}
		//拓扑排序处理 
		toposolve();
	}
} 
```

