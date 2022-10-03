### Question:
The K−P factorization of a positive integer N is to write N as the sum of the P-th power of K positive integers. You are supposed to write a program to find the K−P factorization of N for any positive integers N, K and P.

### Input Specification:

Each input file contains one test case which gives in a line the three positive integers N (≤400), K (≤N) and P (1<P≤7). The numbers in a line are separated by a space.

### Output Specification:

For each case, if the solution exists, output in the format:

```
N = n[1]^P + ... n[K]^P
```

where `n[i]` (`i` = 1, ..., `K`) is the `i`-th factor. All the factors must be printed in non-increasing order.

Note: the solution may not be unique. For example, the 5-2 factorization of 169 has 9 solutions, such as 122+42+22+22+12, or 112+62+22+22+22, or more. You must output the one with the maximum sum of the factors. If there is a tie, the largest factor sequence must be chosen -- sequence { a1​,a2​,⋯,aK​ } is said to be **larger** than { b1​,b2​,⋯,bK​ } if there exists 1≤L≤K such that ai​=bi​ for i<L and aL​>bL​.

If there is no solution, simple output `Impossible`.

### Sample Input 1:

```in
169 5 2
```

### Sample Output 1:

```out
169 = 6^2 + 6^2 + 6^2 + 6^2 + 5^2
```

### Sample Input 2:

```in
169 167 3
```

### Sample Output 2:

```out
Impossible
```

### 思路:
```in

```

### 参考代码:
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PLL pair<LL, LL>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 100;
const int M = 1e5;
const int mod = 1e9 + 7;

int top; //上限
int n,k,p;
bool flag;
int num[444];//直接记录x^p的和
vector<int>path,tmppath;
int ans; //因子最大和
int get(int x)
{
	int res=1;
	for(int i=1;i<=p;i++)
	{
		res*=x;
	}
	num[x]=res;
	return res;
}
void dfs(int u,int cnt,int sum,int maxfac)
{
	if(sum>n)return;
	if(cnt==k)
	{
		if(sum==n && maxfac>ans)
		{
			flag=1;
			ans=maxfac;
			path=tmppath;
		}
		return;
	}

	for(int i=u;i>=1;i--)
	{
		tmppath.push_back(i);
		dfs(i,cnt+1,sum+num[i],maxfac+i);
		tmppath.pop_back();
	}
}


int main()
{
	io;
	cin>>n>>k>>p;
	top=0;
	while(get(top+1)<=n)top++;
	dfs(top,0,0,0);
	if(flag)
	{
		cout<<n<<" = ";
		for(int i=0;i<k;i++)
		{
			if(i!=0)cout<<" + ";
			cout<<path[i]<<"^"<<p;
		}
		cout<<"\n";
	}
	else cout<<"Impossible\n";
	// system("pause");
	return 0;
}
/*

*/

```