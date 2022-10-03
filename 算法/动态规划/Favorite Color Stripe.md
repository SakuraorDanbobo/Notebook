Eva is trying to make her own color stripe out of a given one. She would like to keep only her favorite colors in her favorite order by cutting off those unwanted pieces and sewing the remaining parts together to form her favorite color stripe.

It is said that a normal human eye can distinguish about less than 200 different colors, so Eva’s favorite colors are limited. However the original stripe could be very long, and Eva would like to have the remaining favorite stripe with the maximum length. So she needs your help to find her the best result.

Note that the solution might not be unique, but you only have to tell her the maximum length. For example, given a stripe of colors {2 2 4 1 5 5 6 3 1 1 5 6}. If Eva’s favorite colors are given in her favorite order as {2 3 1 5 6}, then she has 4 possible best solutions {2 2 1 1 1 5 6}, {2 2 1 5 5 5 6}, {2 2 1 5 5 6 6}, and {2 2 3 1 1 5 6}.

### Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N ( ≤ 200 )  which is the total number of colors involved (and hence the colors are numbered from 1 to N). Then the next line starts with a positive integer M ( ≤ 200 ) followed by M MM Eva’s favorite color numbers given in her favorite order. Finally the third line starts with a positive integer L ( ≤ 10e4 ) which is the length of the given stripe, followed by L colors on the stripe. All the numbers in a line a separated by a space.

### Output Specification:
For each test case, simply print in a line the maximum length of Eva’s favorite stripe.

### Sample Input:
6
5 2 3 1 5 6
12 2 2 4 1 5 5 6 3 1 1 5 6

### Sample Output:
7

题目大意：

给出M中颜色作为喜欢的颜色（同时也给出顺序），然后给出一串长度为L的颜色序列，现在要去掉这个序列中的不喜欢的颜色，然后求剩下序列的一个子序列，使得这个子序列表示的颜色顺序符合自己喜欢的颜色的顺序，不一定要所有喜欢的颜色都出现。



```java
#include <bits/stdc++.h>
#define LL long long
#define PII pair<int,int>
#define PSS pair<string,string>
#define x first
#define y second
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e4+5;
const int M=6100;

int a[N],b[N];//颜色的优先级 
int dp[N];
int main()
{
 	
 	int n,m,l,x;
 	scanf("%d %d",&n,&m);
 	for(int i=1;i<=m;i++)
 	{
 		cin>>x;
 		a[x]=i;
	}
	scanf("%d",&m);
	int len=0; 
	while(m--)
	{
		scanf("%d",&x);
		if(a[x]>0)
		{
			b[len++]=a[x];
		}
	}
	int ans=0;
	for(int i=0;i<len;i++)
	{
		dp[i]=1;
		for(int j=0;j<i;j++)
		{
			if(b[j]<=b[i])
			{
				dp[i]=max(dp[i],dp[j]+1);
			}
		}
		ans=max(ans,dp[i]);
	}
	printf("%d",ans);
}
```



