**描述**

任何一个正整数都可以用2的幂次方表示。例如：137=27+23+20

同时约定方次用括号来表示，即ab可表示为a(b)。

由此可知，137可表示为：2(7)+2(3)+2(0)

进一步：7=22+2+20（21用2表示）

3=2+20

所以最后137可表示为：2(2(2)+2+2(0))+2(2+2(0))+2(0)

又如：1315=210+28+25+2+20

所以1315最后可表示为：2(2(2+2(0))+2)+2(2(2+2(0)))+2(2(2)+2(0))+2+2(0)

**输入**

一个正整数n（n≤20000）。

**输出**

一行，符合约定的n的0，2表示（在表示中不能有空格）。

**样例输入**

137

**样例输出**

2(2(2)+2+2(0))+2(2+2(0))+2(0)



```c++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define LL long long
using namespace std;
typedef unsigned long long ull;
const int MOD = 12345;
const int INF = 0x3f3f3f3f;
const int N=1e5+2;
int n,m;
void dfs(int x,int index)
{
	if(x==0)
	{
		return;
	}
	int r=x&1;
	x>>=1;
	dfs(x,index+1); 
	
	if(r!=0 && x!=0)cout<<"+";
	if(r==1)
	{
		if(index==0)cout<<"2(0)";
		else if(index==1)cout<<"2";
		else if(index==2)cout<<"2(2)";
		else
		{
			cout<<"2(";
			dfs(index,0);
			cout<<")";
		}
	}
}
int main()
{
	IO;
	cin>>n;
//	int sum=16384,len=14;
	dfs(n,0);
}

```

