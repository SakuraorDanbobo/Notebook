描述

Marsha and Bill own a collection of marbles. They want to split the collection among themselves so that both receive an equal share of the marbles. This would be easy if all the marbles had the same value, because then they could just split the collection in half. But unfortunately, some of the marbles are larger, or more beautiful than others. So, Marsha and Bill start by assigning a value, a natural number between one and six, to each marble. Now they want to divide the marbles so that each of them gets the same total value.
Unfortunately, they realize that it might be impossible to divide the marbles in this way (even if the total value of all marbles is even). For example, if there are one marble of value 1, one of value 3 and two of value 4, then they cannot be split into sets of equal value. So, they ask you to write a program that checks whether there is a fair partition of the marbles.

输入

Each line in the input describes one collection of marbles to be divided. The lines consist of six non-negative integers n1, n2, ..., n6, where ni is the number of marbles of value i. So, the example from above would be described by the input-line ``1 0 1 2 0 0''. The maximum total number of marbles will be 20000.

The last line of the input file will be ``0 0 0 0 0 0''; do not process this line.

输出

For each colletcion, output ``Collection #k:'', where k is the number of the test case, and then either ``Can be divided.'' or ``Can't be divided.''.

Output a blank line after each test case.

样例输入

1 0 1 2 0 0
1 0 0 0 1 1
0 0 0 0 0 0

样例输出

Collection #1:
Can't be divided.

Collection #2:
Can be divided.



思路：多重部分和问题。 有6种货币，每种货币不超过20000数目，价值从1到6，问你是否能到达一个数num，num=sum/2，sum为所有货币的总价值。



```c++
#include <bits/stdc++.h>
#define x first
#define y second
#define LL long long
#define PII pair<int,int>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=2e5;

int f[7];
int dp[N];

int main()
{
	int pos=1;
	while(cin>>f[1])
	{
		int sum=f[1];
		for(int i=2;i<=6;i++)
		{
			cin>>f[i];
			sum+=f[i]*i;
		}	
		if(sum==0)break;
		
		
		cout<<"Collection #"<< pos++ <<":\n";
		
		//不能平均分 
		if(sum&1)
		{
			cout<<"Can't be divided.\n\n";
			continue;
		}
		
				
		memset(dp,-1,sizeof(dp));
		dp[0]=0;
		for(int i=1;i<=6;i++)
		{
			for(int j=0;j<=sum/2;j++)
			{
				if(dp[j]>=0)
				{
					dp[j]=f[i];
				 } 
				 else if( j<i || dp[j-i]<=0 )dp[j]=-1;
				 else dp[j]=dp[j-i]-1;
			}
		}
		if(dp[sum/2]>=0)cout<<"Can be divided.\n";
		else cout<<"Can't be divided.\n";
		
		cout<<"\n";
	}		
    return 0;
}
```

