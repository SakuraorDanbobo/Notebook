大家倒垃圾的时候，都希望垃圾箱距离自己比较近，但是谁都不愿意守着垃圾箱住。所以垃圾箱的位置必须选在到所有居民点的最短距离最长的地方，同时还要保证每个居民点都在距离它一个不太远的范围内。

现给定一个居民区的地图，以及若干垃圾箱的候选地点，请你推荐最合适的地点。如果解不唯一，则输出到所有居民点的平均距离最短的那个解。如果这样的解还是不唯一，则输出编号最小的地点。

### 输入格式：

输入第一行给出4个正整数：*N*（≤103）是居民点的个数；*M*（≤10）是垃圾箱候选地点的个数；*K*（≤104）是居民点和垃圾箱候选地点之间的道路的条数；*D**S*是居民点与垃圾箱之间不能超过的最大距离。所有的居民点从1到*N*编号，所有的垃圾箱候选地点从*G*1到*GM*编号。

随后*K*行，每行按下列格式描述一条道路：

```
P1 P2 Dist
```

其中`P1`和`P2`是道路两端点的编号，端点可以是居民点，也可以是垃圾箱候选点。`Dist`是道路的长度，是一个正整数。

### 输出格式：

首先在第一行输出最佳候选地点的编号。然后在第二行输出该地点到所有居民点的最小距离和平均距离。数字间以空格分隔，保留小数点后1位。如果解不存在，则输出`No Solution`。

### 输入样例1：

```in
4 3 11 5
1 2 2
1 4 2
1 G1 4
1 G2 3
2 3 2
2 G2 1
3 4 2
3 G3 2
4 G1 3
G2 G1 1
G3 G2 2
```

### 输出样例1：

```out
G1
2.0 3.3
```

### 输入样例2：

```
2 1 2 10
1 G1 9
2 G1 20
```

### 输出样例2：

```
No Solution
```



```c++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define LL long long
using namespace std;
const int MOD = 1e9+7;
const int INF = 0x3f3f3f3f;
const int N =1e3+30;
int n,m,k,far;
int ma[N][N];
int vis[N];
int d[N];
int get(string s)
{
	//垃圾桶 
	if(s[0]=='G')
	{
		return atoi(s.substr(1).c_str())+n; //接在居民n号后排序 
	}
	else //居民 
	{
		return atoi(s.c_str());
	}
}
void disjk(int x)
{
	//初始化 
	memset(d,INF,sizeof(d));
	memset(vis,0,sizeof(vis));
	d[x]=0;
	while(1)
	{
		int u=-1;
		//从未选取的点中找一个离起点最近的点，这里要包括垃圾桶待选点
		for(int i=1;i<=n+m;i++)
		{
			if(!vis[i] && (u==-1 || d[i]<d[u]))u=i; 
		}
		if(u==-1)return;
		vis[u]=1;
		//更新以这个点出发的距离
		for(int i=1;i<=n+m;i++)
		{
			if(d[i]>d[u]+ma[u][i])
			{
				d[i]=d[u]+ma[u][i];
			}
		 } 
	} 
}
int main()
{
	IO;
 	cin>>n>>m>>k>>far;
 	string a,b;
 	int q,w,v;
 	//处理边的关系 
 	memset(ma,INF,sizeof(ma));
 	while(k--)
 	{
 		cin>>a>>b>>v;
 		q=get(a);
 		w=get(b);
 		ma[q][w]=ma[w][q]=v;
	}
	int ansnum=0;
	double avgminc=INF; //最短平均距离 
	double mmin=0; //垃圾桶中最短距离的当中最长的一个
	int ans; //存储最佳地点原下标 
	int exit=0;  //是否存在合适的垃圾桶点 
	//找垃圾桶，遍历每个垃圾桶和居民区的距离 
	for(int i=n+1;i<=n+m;++i)
	{
		disjk(i);
		int flag=1;
		double sum=0; //sum储存每个垃圾箱到所有住户的最短距离
		double cmin=INF; //当前最小距离点 
		
		//判断居民区是否超过最大距离，懒哎 
		for(int j=1;j<=n;j++)
		{
			if(d[j]>far)
			{
				flag=0;
				break;
			}
			if(d[j]<cmin)cmin=d[j];
			sum+=d[j]; 
		}
		if(!flag)continue;
		exit=1;
		sum/=n; //平均值
		if(cmin>mmin) 
		{
			avgminc=sum;
			ans=i-n;
			mmin=cmin; 
		}
		else if(cmin==mmin && avgminc>sum) //最短距离最长的地方相同，比较平均距离 
		{
			avgminc=sum;
			ans=i-n;
			mmin=cmin;
		}
	} 
	if(exit)
	{
		printf("G%d\n",ans);
		printf("%.1f %.1f",mmin,avgminc);
	}
	else cout<<"No Solution\n";
	return 0;
}
 
```

