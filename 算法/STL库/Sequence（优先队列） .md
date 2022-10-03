描述

Given m sequences, each contains n non-negative integer. Now we may select one number from each sequence to form a sequence with m integers. It's clear that we may get n ^ m this kind of sequences. Then we can calculate the sum of numbers in each sequence, and get n ^ m values. What we need is the smallest n sums. Could you help us?

输入

The first line is an integer T, which shows the number of test cases, and then T test cases follow. The first line of each case contains two integers m, n (0 < m <= 100, 0 < n <= 2000). The following m lines indicate the m sequence respectively. No integer in the sequence is greater than 10000.

输出

For each test case, print a line with the smallest n sums in increasing order, which is separated by a space.

样例输入

1
2 3
1 2 3
2 2 3


样例输出

3 3 4



思路 :   要想找到n行序列的最小和，那么就得求得n-1行序列 ... 直到第2行序列。因此我们可以每合并两行，得到 n 个最小和作为新的一行序列继续合并下去，直到唯一的一行，则为最后答案。我们用优先队列来维护n个最小数和。

```c++
#include <bits/stdc++.h>
#define x first
#define y second
#define LL long long
#define PII pair<int,int>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=2e5;
int m,n;
int f[2002],g[2002];

void add()
{
	priority_queue<int> p;
	
	for(int i=0;i<n;i++)
	{
		p.push(f[0]+g[i]);
	}
	
	for(int i=1;i<n;i++)
	{
		for(int j=0;j<n;j++)
		{
			int ans=f[i]+g[j];
			if( ans < p.top() )
			{
				p.pop();
				p.push(ans);
			}
			else break;
		}
	}
	
	for(int i=0;i<n;i++)
	{
		f[n-i-1]=p.top();
		p.pop();
	}
}
int main()
{
	int t;
	cin>>t;
	while(t--)
	{
		cin>>m>>n;
		for(int i=0;i<n;i++)
		{
			cin>>f[i];
		}
		sort(f,f+n);
		for(int i=1;i<m;i++)
		{
			for(int j=0;j<n;j++)cin>>g[j];
			sort(g,g+n);
			add(); 
		}
		for(int i=0;i<n;i++)
		{
			if(i!=0)cout<<" ";
			cout<<f[i];
		}
		cout<<"\n";
	 } 
    return 0;
}
/*
1
2 3
1 2 3
3 4 5
*/
```

