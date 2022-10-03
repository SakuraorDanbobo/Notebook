**描述**

The inversion number of a given number sequence a1, a2, ..., an is the number of pairs (ai, aj) that satisfy i < j and ai > aj.

For a given sequence of numbers a1, a2, ..., an, if we move the first m >= 0 numbers to the end of the seqence, we will obtain another sequence. There are totally n such sequences as the following:

a1, a2, ..., an-1, an (where m = 0 - the initial seqence)
a2, a3, ..., an, a1 (where m = 1)
a3, a4, ..., an, a1, a2 (where m = 2)
...
an, a1, a2, ..., an-1 (where m = n-1)

You are asked to write a program to find the minimum inversion number out of the above sequences.

**输入**

The input consists of a number of test cases. Each case consists of two lines: the first line contains a positive integer n (n <= 5000); the next line contains a permutation of the n integers from 0 to n-1.

**输出**

For each case, output the minimum inversion number on a single line.

**样例输入**

10
1 3 6 9 0 8 5 7 4 2

**样例输出**

16

 

```C++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define LL long long
#define PI 3.1415926
using namespace std;
const int MOD = 1e9+7;
const int INF = 0x3f3f3f3f;
const int N=1e4+2;
int a[5002],c[5002];
int n;
void add(int x)
{
	while(x<=n)
	{
//		cout<<x<<"\n";
		c[x]++;
//		x&(-x)//取最低位的1 
		x+=(x&(-x));
		
	}
}
LL get(int x)
{
	LL ans=0;
	while(x>0)
	{
		ans+=c[x];
		x-=(x&(-x));
	}
	return ans;
}
int main()
{
//	IO;
	while(cin>>n)
	{
		memset(c,0,sizeof(c));
		LL ans=0; 
		for(int i=1;i<=n;++i)
		{
			cin>>a[i];
			a[i]++; //+1，下标从1开始 
			add(a[i]); 
			ans+=(i-get(a[i]));  
		}	
		LL res=ans;
		for(int i=1;i<n;i++)
		{
			ans+=(n-a[i]-(a[i]-1));
			if(res>ans)res=ans;
		}
		cout<<res<<"\n";
	}
	
}
 
 
 
```

