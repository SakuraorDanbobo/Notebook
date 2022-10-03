#### Question
The only difference between the two versions is that in this version n≤2⋅10^5 and the sum of n over all test cases does not exceed 2⋅10^5.

A terminal is a row of n equal segments numbered 1 to n in order. There are two terminals, one above the other.

You are given an array a of length n. For all i=1,2,…,n, there should be a straight wire from some point on segment i of the top terminal to some point on segment ai of the bottom terminal. You can't select the endpoints of a segment. For example, the following pictures show two possible wirings if n=7 and a=[4,1,4,6,7,7,5].
![[Pasted image 20220514173153.png]]
A crossing occurs when two wires share a point in common. In the picture above, crossings are circled in red.

What is the maximum number of crossings there can be if you place the wires optimally?

#### Input
The first line contains an integer t (1≤t≤1000) — the number of test cases.

The first line of each test case contains an integer n (1≤n≤2⋅10^5) — the length of the array.

The second line of each test case contains n integers a1,a2,…,an (1≤ai≤n) — the elements of the array.

The sum of n across all test cases does not exceed 2⋅10^5.

#### Output
For each test case, output a single integer — the maximum number of crossings there can be if you place the wires optimally.

#### input
```in
4
7
4 1 4 6 7 7 5
2
2 1
1
1
3
2 2 2
```
#### output
```out
6
1
0
3
```
#### Note
The first test case is shown in the second picture in the statement.
In the second test case, the only wiring possible has the two wires cross, so the answer is 1.
In the third test case, the only wiring possible has one wire, so the answer is 0.

#### 题意：
```in
给你n条线段，问两线段间最多相交的点数。
```
#### 思路：
```in
两线段相交需要满足x1<x2,y1>y2。
本题需统计的就是每个点的下端有多少个点小于等于。
因为n个点的上端是有序的，因此我们对下端从后往前使用树状数组进行计算和统计。
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
const int add = 1e6;
int t, n, m, k;

int f[N],sum[N];
void insert(int x)
{
	for(;x<=n;x+=x&(-x))sum[x]++;
}
LL cal(int x)
{
	LL ans=0;
	for(;x;x-=x&(-x))ans+=sum[x];
	return ans;
}

int main()
{
	// io;
	cin>>t;
	while(t--)
	{
		cin>>n;
		LL res=0;
		memset(sum,0,sizeof sum);
		for(int i=1;i<=n;i++)cin>>f[i];
		for(int i=n;i>=1;i--)
		{
			res+=cal(f[i]);
			insert(f[i]);
		}
		cout<<res<<"\n";
	}	

	system("pause");
	return 0;
}
/*
1110100011
*/

```