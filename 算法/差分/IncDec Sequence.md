

描述

给定一个长度为 n(n≤10^5 ) 的数列 {a_1,a_2,…,a_n}，每次可以选择一个区间 [l,r]，使下标在这个区间内的数都加一或者都减一。
求至少需要多少次操作才能使数列中的所有数都一样，并求出在保证最少次数的前提下，最终得到的数列可能有多少种。

输入

第一行一个正整数n。
接下来n行，每行一个整数，第i+1行的整数表示ai。

输出

第一行输出最少操作次数。
第二行输出最终能得到多少种结果。

样例输入

4
1
1
2
2


样例输出

1
2





```c++
#include <bits/stdc++.h>
#define x first
#define y second
#define LL long long
#define PII pair<int,int>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e5+5;
const int M=1e7;
LL f[N]; 

/*
	差分思路：
	对于差分后的序列，每次对[l,r]进行+1/-1，相当于在差分后的数组上对l进行+1/-1，然后对r+1进行-1/+1 
	当r=n时，就相当于对l进行了+1/-1。
	那么，我们只需要保留第一个位置的数字，对2~n上的差分序列值为0即可。 
*/ 
int main()
{
	 int n;
	 cin>>n;
	 for(int i=1;i<=n;i++)cin>>f[i];
	 
	 LL a=0; //正差分和 
	 LL b=0; //负差分和 
	 for(int i=2;i<=n;i++)
	 {
	 	LL d=f[i]-f[i-1];
	 	if(d>0)a+=d;
	 	else b-=d;
	 }
	 cout<<max(a,b)<<"\n";
	 cout<<abs(a-b)+1;
}
/*

*/
```

