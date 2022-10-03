Question

Bethany would like to tile her bathroom. The bathroom has width w centimeters and length l centimeters. If Bethany simply used the basic tiles of size 1×1 centimeters, she would use w⋅l of them.

However, she has something different in mind.

On the interior of the floor she wants to use the 1×1 tiles. She needs exactly (w−2)⋅(l−2) of these.
On the floor boundary she wants to use tiles of size 1×a for some positive integer a. The tiles can also be rotated by 90 degrees.
For which values of a can Bethany tile the bathroom floor as described? Note that a can also be 1.

Input
Each test contains multiple test cases. The first line contains an integer t (1≤t≤100) — the number of test cases. The descriptions of the t test cases follow.

Each test case consist of a single line, which contains two integers w, l (3≤w,l≤109) — the dimensions of the bathroom.

Output
For each test case, print an integer k (0≤k) — the number of valid values of a for the given test case — followed by k integers a1,a2,…,ak (1≤ai) — the valid values of a. The values a1,a2,…,ak have to be sorted from smallest to largest.

It is guaranteed that under the problem constraints, the output contains at most 200000 integers.

Example
inputCopy
3
3 5
12 12
314159265 358979323
outputCopy
3 1 2 3
3 1 2 11
2 1 2
 

思路：对墙壁4边的总个数sum求因数，判断因数是否满足条件即可。

参考代码：

```c++
#include <bits/stdc++.h>
 
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long
#define x first
#define y second
#define PII pair<int,int>
#define PDD pair<double,double>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=2e5+5;
const int M=1e7;
const int mod=1e9+7;

//3种成立的情况判断
bool ok(int x,int w,int h)
{
	if(w%x==1 && h%x==1)return true;
	
	if(w%x==0 && (h-2)%x==0)return true;
	if((w-2)%x==0 && h%x==0)return true;
	
	//3 4
	if((w-2)%x==0 && w%x==0 && h%x==1)return true;
	if((h-2)%x==0 && h%x==0 && w%x==1)return true;
	return false;
}
int main()
{
	LL t,w,l,k;
	set<int> s;
	set<int>::iterator it;
	while(~scanf("%lld",&t))
	{
		while(t--)
		{
			s.clear();
			scanf("%lld%lld",&w,&l);
			s.insert(1);
			k=(w-2)*2+l*2;
			for(int i=2;i<=sqrt(k);i++)
			{
				if(k%i==0)
				{
					if(ok(i,w,l))
					{
						s.insert(i);
					}
					if(ok(k/i,w,l))
					{
						s.insert(k/i);
					}
				}
			}
			printf("%d",s.size());
			for(it=s.begin();it!=s.end();++it)
			{
				printf(" %d",*it);
			}
			printf("\n");
		}	
	}
}
```

