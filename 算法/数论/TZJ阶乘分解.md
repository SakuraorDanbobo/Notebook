给定整数 N，试把阶乘 N!分解质因数，按照算术基本定理的形式输出分解结果中的 pi 和 ci 即可。

#### 输入格式

一个整数 N。

#### 输出格式

N! 分解质因数后的结果，共若干行，每行一对 pi,ci，表示含有 pcii 项。按照 pi 从小到大的顺序输出。

#### 数据范围

3≤N≤10e6

#### 输入样例：

```
5
```

#### 输出样例：

```
2 3
3 1
5 1
```

#### 样例解释

5!=120=2^3∗3∗5



```c++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define LL long long
using namespace std;
typedef unsigned long long ull;
const int MOD = 10000;
const int INF = 0x3f3f3f3f;
const int N=1e6+2;
int pri[N];
int vis[N];
int cnt;
void init(int n)
{
	for(int i=2;i<=n;++i)
	{
		if(!vis[i])pri[cnt++]=i;
		for(int j=0;j<cnt && pri[j]*i<=n;++j)
		{
			vis[pri[j]*i]=1;
			if(i%pri[j]==0)break;
		}
	}
}
int main()
{
	IO; 
	int n;
	cin>>n;
	//打2~n内的质数表 
	init(n);
	//判断n中含有多少个质因数
	// 即 [n/p] 向下取整 的个数。 
	for(int i=0;i<cnt;++i)
	{
		//例如10 中，质因数2 ,那么 2,4,8都是包含质因数的 
		LL p=pri[i];
		int sum=0; //累计个数 
		while(p<=n)
		{
			sum+=n/p;
			p*=pri[i];
		}
		cout<<pri[i]<<" "<<sum<<"\n";
	}
}
 
```



