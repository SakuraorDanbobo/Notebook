**题目描述**

莲子需要维护一些**可重集**。初始时，有 n 个元素，每个元素有一个初始值，且每个元素构成一个单元素的可重集。

接下来有 m 次操作，分为如下 3 种：

1 x y: 将 x 所在的集合与 y 所在的集合合并。

2 x k: 将元素 x 的值增加 k。

3 x: 询问 x 所在的集合的全部子集的元素和之**和，结果对 10e9+7 取模。

**输入**



第一行两个整数 n,m （1≤n,m≤10e6）。 

第二行 n 个整数，第i个整数，表示第i个元素的初始值ai(|ai|≤10e9)。 

接下来 m 行每行若干个整数，表示一个操作，操作2中|k|≤10e9。 

**输出**

对于每一个的询问，都有一行输出，每行包含一个整数，表示询问的结果对 10e9+7 取模的值。 

**样例输入**

3 5
1 2 3
3 1
1 1 2
3 1
2 1 1
3 2

**样例输出**

1
6
8

**提示**

对于第一个询问，集合{1}的子集为 空集 和 {1}。其元素和分别为 0 和 1。故其全部子集的元素和之和为 0+1=1。

对于第二个询问，集合{1,2}的子集为 空集 ，{1} ，{2} 和 {1,2}。其元素和分别为 0 , 1 , 2 和 3。故其全部子集的元素和之和为 0+1+2+3=6 。

对于第三个询问，集合{1,3} 的子集为 空集 ，{1} ，{3} 和 {1,3}。其元素和分别为 0 , 1 , 3 和 4。故其全部子集的元素和之和为 0+1+3+4=8 。



**思路** ： 一个大小为k个的S全部子集的元素和为： 2^(k-1) * ∑Si 。(即每个元素出现在子集的个数) ，因此我们在对元素x进行并查集的时候，开辟cnt 数组 存储元素个数 ，开辟 sum 数组存储所有元素和。需要注意的就是在取余的操作过程中，负数的处理。



参考代码：

```c++
#include <iostream>
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long
#define x first
#define y second
#define PII pair<int,int>
#define PDD pair<double,double>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6+5;
const int M=1e7;
const int mod=1e9+7;

int pre[N];
int cnt[N];
LL ans[N];
int n,m,x,a,b;
int find(int u)
{
	if(pre[u]!=u)
	{
		pre[u]=find(pre[u]);
	}
	return pre[u];
}
LL get(int k)
{
	LL sum=1,a=2;
	k--;
	while(k)
	{
		if(k&1)sum=sum*a%mod;
		a=a*a%mod;
		k>>=1;
	}
	return sum;
}
//一个大小为k的S全部子集的元素和为： 2^(k-1) * ∑Si 。(即每个元素出现在子集的个数) 
int main()
{
	io;
	 cin>>n>>m;
	 for(int i=1;i<=n;i++)
	 {
	 	cin>>ans[i];
	 	pre[i]=i;
	 	cnt[i]=1;
	 }
	 
	 int l,r;
	 while(m--)
	 {
	 	cin>>x;
//	 	for(int i=1;i<=n;i++)
//	 	{
//	 		cout<<"当前:"<<i<<" 父节点: "<<find(i)<<" ans: "<<ans[find(i)]<<"\n";
//		 }
//		 cout<<"------\n";
	 	if(x==1) //合并 
	 	{
	 		cin>>a>>b;
	 		l=find(a);
	 		r=find(b);
	 		if(l==r)continue;
	 		pre[r]=l;
	 		
	 		ans[l]=((ans[l]+ans[r])%mod+mod)%mod;
	 		cnt[l]+=cnt[r];
		}
		else if(x==2)
		{
			cin>>a>>b;
			l=find(a);
			ans[l]=((ans[l]+b)%mod+mod)%mod;
		}
		else
		{
			cin>>a;
			l=find(a);
			cout<<(get(cnt[l])*ans[l]%mod+mod)%mod<<"\n";
		}
	 }
}

```

