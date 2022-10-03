描述

Now I think you have got an AC in Ignatius.L's "Max Sum" problem. To be a brave ACMer, we always challenge ourselves to more difficult problems. Now you are faced with a more difficult problem.

Given a consecutive number sequence S1, S2, S3, S4 ... Sx, ... Sn (1 ≤ x ≤ n ≤ 1,000,000, -32768 ≤ Sx ≤ 32767). We define a function sum(i, j) = Si + ... + Sj (1 ≤ i ≤ j ≤ n).

Now given an integer m (m > 0), your task is to find m pairs of i and j which make sum(i1, j1) + sum(i2, j2) + sum(i3, j3) + ... + sum(im, jm) maximal (ix ≤ iy ≤ jx or ix ≤ jy ≤ jx is not allowed).

But I`m lazy, I don't want to write a special-judge module, so you don't have to output m pairs of i and j, just output the maximal summation of sum(ix, jx)(1 ≤ x ≤ m) instead. ^_^
输入

Each test case will begin with two integers m and n, followed by n integers S1, S2, S3 ... Sn.
Process to the end of file.

输出

Output the maximal summation described above in one line.

样例输入

1 3 1 2 3
2 6 -1 4 -2 3 -2 3

样例输出

6
8

 

```
题意：将一个长度为n的序列，分成m段不相交叉的子段，使得他们的和最大。

状态定义：dp[i][j], 前j个数中,以S[j]结尾，分i段的最大和。
状态转移：dp[i][j]=max(dp[i-1][k]+a[j],dp[i][j-1]+a[j])  (i-1< k< j-1)
因为本题n为1e6次，所以需要降维。
因为每次状态转移只需要用到前一层的状态，

因此，状态定义：
dp[N]; // dp[j] , 前j个数以S[j]结尾 的最大和 
pre[N];// 记录上一层分i段时，1~j-1的最大值 
则：状态转移：
dp[j]=max(pre[j-1],dp[j-1])+s[j];
```



```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long
#define PII pair<int,int>
#define PSS pair<string,string>
#define x first
#define y second
using namespace std;
typedef unsigned long long ull;
const int INF=0x3f3f3f3f;
const int N=1e6+7;
const int M=6100;

int n,m,s[N];
//https://blog.51cto.com/u_13688928/2117013
//dp[i][j], 前j个数中,以S[j]结尾，分i段的最大和。
 
int dp[N]; // dp[j] , 前j个数以S[j]结尾 的最大和 
int pre[N];// 记录上一层分i段时，1~j-1的最大值 
int main()
{
	 while(~scanf("%d%d",&m,&n))
	 {
	 	for(int i=1;i<=n;i++)scanf("%d",&s[i]);
	 	memset(dp,0,sizeof dp);
	 	memset(pre,0,sizeof pre);
	 	//划分段数 
	 	int ans;
	 	for(int i=1;i<=m;i++)
		{
			ans=-INF;
			//枚举终点 
			for(int j=i;j<=n;j++)
			{
				dp[j]=max(pre[j-1],dp[j-1])+s[j];
				pre[j-1]=ans;
				ans=max(ans,dp[j]);
		    } 	
		} 
	 	printf("%d\n",ans);
	 }
}
/*
*/
```

