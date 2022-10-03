> 题目地址：https://codeforces.com/contest/1615/problem/B

**Description**

You are given an array consisting of all integers from [ l，r ] inclusive. For example, if l=2 and r=5, the array would be [2,3,4,5]， What's the minimum number of elements you can delete to make the [bitwise AND](https://en.wikipedia.org/wiki/Bitwise_operation#AND) of the array non-zero?

A *bitwise AND* is a binary operation that takes two equal-length binary representations and performs the *AND* operation on each pair of the corresponding bits.

**Input**

The first line contains one integer t (1≤t≤10e4) — the number of test cases. Then t cases follow.

The first line of each test case contains two integers ll and rr (1≤l≤r≤2⋅10e5) — the description of the array.

**Output**

For each test case, output a single integer — the answer to the problem.

**Sample Input**

5
1 2
2 8
4 5
1 5
100000 200000

**Sample Output**

1
3
0
2
31072

**Note**

In the first test case, the array is [1,2]. Currently, the *bitwise AND* is 0, as 1 & 2=0. However, after deleting 1 (or 2), the array becomes [2][2] (or [1][1]), and the *bitwise AND* becomes 2 (or 1). This can be proven to be the optimal, so the answer is 1.

In the second test case, the array is [2,3,4,5,6,7,8]. Currently, the *bitwise AND* is 0. However, after deleting 4, 5, and 8, the array becomes [2,3,6,7], and the *bitwise AND* becomes 2. This can be proven to be the optimal, so the answer is 3. Note that there may be other ways to delete 3 elements.

题目意思：给定一个区间，l连续到r的数字，求区间内至少移走几个数字，使剩下所有数字与（And，&)和值不为0.

思路：打表求所有数二进制下每一位1的总和。（即打表前缀和）

代码如下：

```c++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define LL long long
using namespace std;
const int N = 2e5+5;
const int g = 19;
int ans[g][N];
void init()
{
	//打表前缀和 
	//二进制位下最多18位 
	for(int i=0;i<g;++i)
	{
		for(int j=1;j<N;j++)
		{
			//(j>>i)&1 判断第i位是否为1 
			ans[i][j]+=ans[i][j-1] + ((j>>i)&1);
		}
	}
}
int main()
{
	init();
	int t,l,r;
	scanf("%d",&t);
	while(t--)
	{
		scanf("%d %d",&l,&r);
		int res=r-l+1;
		for(int i=0;i<g;++i)
		{
			//1的个数 
			int k=ans[i][r]-ans[i][l-1];
			//总个数减去1的个数 
			res=min(res,(r-l+1)-k);
		}
		printf("%d\n",res);
	}
}
```

