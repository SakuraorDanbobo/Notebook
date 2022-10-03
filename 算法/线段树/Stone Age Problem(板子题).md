#### Question
Once upon a time Mike and Mike decided to come up with an outstanding problem for some stage of ROI (rare olympiad in informatics). One of them came up with a problem prototype but another stole the idea and proposed that problem for another stage of the same olympiad. Since then the first Mike has been waiting for an opportunity to propose the original idea for some other contest... Mike waited until this moment!

You are given an array a of n integers. You are also given q queries of two types:

Replace i-th element in the array with integer x.
Replace each element in the array with integer x.
After performing each query you have to calculate the sum of all elements in the array.

#### Input
The first line contains two integers n and q (1≤n,q≤2⋅10^5) — the number of elements in the array and the number of queries, respectively.

The second line contains n integers a1,…,an (1≤ai≤10^9) — elements of the array a.

Each of the following q lines contains a description of the corresponding query. Description begins with integer t (t∈{1,2}) which denotes a type of the query:

If t=1, then two integers i and x are following (1≤i≤n, 1≤x≤10^9) — position of replaced element and it's new value.
If t=2, then integer x is following (1≤x≤10^9) — new value of each element in the array.
#### Output
Print q integers, each on a separate line. In the i-th line print the sum of all elements in the array after performing the first i queries.

#### input
5 5
1 2 3 4 5
1 1 5
2 10
1 5 11
1 4 1
2 1
#### output
19
50
51
42
5

#### 题意：
```in
给你n个初始序列。有2个操作:
1.修改一个点值
2.修改所有区间的值
每次操作完输出所有和
```
#### 参考代码：
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
int t, n, m, k;
LL f[N],g[4*N],add[4*N];

void pushup(int rt)
{
	g[rt]=g[rt<<1]+g[rt<<1|1];
}
void create(int l,int r,int rt)
{
	if(l==r)
	{
		g[rt]=f[l];
		return;
	}
	int mid=(l+r)>>1;
	create(l,mid,rt<<1);
	create(mid+1,r,rt<<1|1);

	pushup(rt);
}
void pushdown(int rt,LL ln,LL rn)
{
	if(add[rt])
	{
		add[rt<<1]=add[rt];
		add[rt<<1|1]=add[rt];

		g[rt<<1]=add[rt]*1L*ln;
		g[rt<<1|1]=add[rt]*1L*rn;

		add[rt]=0;
	}
}
void update(int L,LL C,int l ,int r,int rt)
{
	if(l==r)
	{
		g[rt]=C;
		return;
	}
	int mid=(l+r)>>1;
	pushdown(rt,mid-l+1,r-mid);
	if(L<=mid)update(L,C,l,mid,rt<<1);
	else update(L,C,mid+1,r,rt<<1|1);

	pushup(rt);
}

void update1(int L,int R,LL C,int l ,int r,int rt)
{
	if(L<=l && R>=r)
	{
		g[rt]=C*(r-l+1);
		add[rt]=C;
		return;
	}
	int mid=(l+r)>>1;
	pushdown(rt,mid-l+1,r-mid);
	if(L<=mid)update1(L,R,C,l,mid,rt<<1);
	if(R>mid) update1(L,R,C,mid+1,r,rt<<1|1);
	
	pushup(rt);
}
LL query(int L ,int R,int l,int r,int rt)
{
	if(L<=l && R>=r)
	{
		return g[rt];
	}
	int mid=(l+r)>>1;
	pushdown(rt,mid-l+1,r-mid);

	LL ans=0;
	if(L<=mid)ans+=query(L,R,l,mid,rt<<1);
	if(R>mid)ans+=query(L,R,mid+1,r,rt<<1|1);

	return ans;
}
int main()
{
	io;
	LL n,q,t,a,b;
	cin>>n>>q;
	for(int i=1;i<=n;i++)
	{
		cin>>f[i];
	}
	create(1,n,1);
	while(q--)
	{
		cin>>t;
		if(t==1)
		{
			cin>>a>>b;
			update(a,b,1,n,1);
		}
		else 
		{
			cin>>a;
			update1(1,n,a,1,n,1);
		}
		cout<<query(1,n,1,n,1)<<"\n";
	}
	 

	system("pause");
	return 0;
}
/*
 
*/

```