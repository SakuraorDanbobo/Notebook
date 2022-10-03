**描述**

levil特别喜欢数学，12的因子有1 2 3 4 6，他的因子和为16。如果这个数字很大呢，levil当然也不想口算，他会用程序算出来。但是如果算连续的几个呢，好像程序会超时哦，所以这个任务就交给你了。
现在定义f[i]表示i的因子的和，请你帮忙算下f[a] + f[a + 1] + f[a + 2] + ... + f[b]的值。
数字n本身不算自己的因子，1算自己的因子；特别的，如果这个数是1，那么因子就是1。

**输入**

输入仅一行，为两个整数a,b。(1 <= a <= b <= 107)

**输出**

输出一行，为f[a] + f[a + 1] + f[a + 2] + ... + f[b]的值。

**样例输入**

2 4

**样例输出**

5

```c++
#include <iostream>
#include <stdio.h> 
#include <algorithm>
#define LL long long
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
using namespace std;
const int MOD = 1e7+7;
const int INF = 0x3f3f3f3f;
const int MAXN = 1e7+5;
int main()
{
	int a,b;
	scanf("%d %d",&a,&b);
	//ans初始值为因数是1的和
	LL ans=b-a+1;  
	// 计算每个倍数出现的次数
	for(int i=2;i<=b;i++)
	{
		//前缀和相减，b前i倍个数，a前i倍个数(不包括a) 
		ans+=i*(b/i-(a-1)/i);
		//减去因数等于自身数
		if(i>=a && i<=b)ans-=i; 
	} 
	printf("%lld",ans);
	
}

```

