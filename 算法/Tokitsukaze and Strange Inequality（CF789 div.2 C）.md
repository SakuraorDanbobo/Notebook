Tokitsukaze has a permutation p of length n. Recall that a permutation p of length n is a sequence p1,p2,…,pn consisting of n distinct integers, each of which from 1 to n (1≤pi≤n).

She wants to know how many different indices tuples [a,b,c,d] (1≤a<b<c<d≤n) in this permutation satisfy the following two inequalities:

pa\<pc and pb\>pd.
Note that two tuples [a1,b1,c1,d1] and [a2,b2,c2,d2] are considered to be different if a1≠a2 or b1≠b2 or c1≠c2 or d1≠d2.

Input
The first line contains one integer t (1≤t≤1000) — the number of test cases. Each test case consists of two lines.

The first line contains a single integer n (4≤n≤5000) — the length of permutation p.

The second line contains n integers p1,p2,…,pn (1≤pi≤n) — the permutation p.

It is guaranteed that the sum of n over all test cases does not exceed 5000.

Output
For each test case, print a single integer — the number of different [a,b,c,d] tuples.

Example
input
3
6
5 3 6 1 4 2
4
1 2 3 4
10
5 1 6 2 8 3 4 10 9 7
output
3
0
28
Note
In the first test case, there are 3 different [a,b,c,d] tuples.

p1=5, p2=3, p3=6, p4=1, where p1<p3 and p2>p4 satisfies the inequality, so one of [a,b,c,d] tuples is [1,2,3,4].

Similarly, other two tuples are [1,2,3,6], [2,3,5,6].

```in
题解：https://codeforces.com/contest/1678/attachments/download/16089/Codeforces%20Round%20789%20Chinese%20Tutorial.pdf
```

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
const int M = 1e6;
const int mod = 1e9 + 7;
const int add = 1e6;

int t, n, m;
LL f[5555], ac[5555];
void solve1()
{
	memset(ac, 0, sizeof ac);
	LL ans = 0;
	for (int i = 1; i <= n; i++)
	{
		ac[i] = 0;
		LL left = 0;
		for (int j = 1; j < i; j++)
		{
			/*
			ac[j] , how many p < q
			0..[...p...]..j..[...q...]..i
			*/
			if (f[j] > f[i])
				ans += ac[j];
			ac[j] += left;
			if (f[j] < f[i])
				left++;
		}
	}
	cout << ans << "\n";
}
LL fb[5555]; // pb>pd的个数
LL sumf[5555];//前缀和
void solve2()
{
	memset(fb,0,sizeof fb);
	//枚举每一个b，[b+1,n]中pb>pd的个数
	for(int i=1;i<=n;i++)
	{
		for(int j=i+1;j<=n;j++)
		{
			if(f[i]>f[j])fb[i]++;
		}
	}
	LL ans=0;
	//枚举c区间
	for(int k=1;k<=n;k++)
	{
		//减去c与d相等的情况
		for(int j=1;j<k;j++)
		{
			if(f[j]>f[k])fb[j]--;
		}

		//求前缀和
		for(int i=1;i<=k;i++)sumf[i]=sumf[i-1]+fb[i];

		//统计结果
		//枚举a区间
		for(int i=1;i<k;i++)
		{
			//加上区间[a+1,c-1]中pb>pd的种数
			if(f[i]<f[k])ans+=sumf[k-1]-sumf[i];
		}
	}

	cout<<ans<<"\n";
}
int main()
{
	io;
	cin >> t;
	while (t--)
	{
		cin >> n;
		for (int i = 1; i <= n; i++)cin >> f[i];
		// solve1();
		solve2();
	}
}
```