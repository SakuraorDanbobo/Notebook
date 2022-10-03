**描述**

一个给定的正整数序列，在每个数之前都插入+号或−号后计算它们的和。比如序列：1、2、4共有8种可能的序列：

(+1) + (+2) + (+4) = 7
(+1) + (+2) + (-4) = -1
(+1) + (-2) + (+4) = 3
(+1) + (-2) + (-4) = -5
(-1) + (+2) + (+4) = 5
(-1) + (+2) + (-4) = -3
(-1) + (-2) + (+4) = 1
(-1) + (-2) + (-4) = -7

所有结果中至少有一个可被整数k整除，我们则称此正整数序列可被k整除。例如上述序列可以被3、5、7整除，而不能被2、4、6、8……整除。注意：0、−3、−6、−9……都可以认为是3的倍数。

**输入**

输入的第一行包含两个数：N(2<N<10000)和k(2<=k<100)，其中N代表一共有N个数，k代表被除数。第二行给出序列中的N个整数，这些整数的取值范围都0到10000之间（可能重复）。

**输出**

如果此正整数序列可被k整除，则输出YES，否则输出NO。（注意：都是大写字母）

**样例输入**

3 2

1 2 4

**样例输出**

NO

参考：https://blog.csdn.net/m0_37579232/article/details/84994449

```C++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
using namespace std;
typedef unsigned long long ull;
typedef long long LL;
const int MOD = 1e9+7;
const int INF = 0x3f3f3f3f;
const int N =1e4+30;
int f[N];
int dp[N][100]; //dp[i][j]:前i个数字模k是否等于j 
int main()
{
	IO;
	int n,k;
	cin>>n>>k;
	for(int i=1;i<=n;++i)
	{
		cin>>f[i];
	}
	//0模任何数，余数都为0 
	dp[0][0]=1; 
	for(int i=1;i<=n;++i) //前i个数 
	{
		for(int j=0;j<k;++j) //余数j 
		{
			//+的情况，(a+b)%k ?= j  ===> (yes) a%k=j-b%k
			//为防止下标变负，j-f[i]%k ===> (k+j-f[i]%k)%k 
//			dp[i][j]=dp[i-1][(k+j-f[i]%k)%k];
			
			//-的情况  (a-b)%k ?= j  ===> (yes) a%k=(j+b%k)%k
//			dp[i][j]=dp[i-1][(j+f[i]%k)%k]; 
			
			dp[i][j]=dp[i-1][(k+j-f[i]%k)%k] || dp[i-1][(j+f[i]%k)%k];
		}
	}
	if(dp[n][0])cout<<"YES";
	else cout<<"NO";
}
 
```



