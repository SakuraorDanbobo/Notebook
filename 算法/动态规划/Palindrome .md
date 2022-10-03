描述

A palindrome is a symmetrical string, that is, a string read identically from left to right as well as from right to left. You are to write a program which, given a string, determines the minimal number of characters to be inserted into the string in order to obtain a palindrome.

As an example, by inserting 2 characters, the string "Ab3bd" can be transformed into a palindrome ("dAb3bAd" or "Adb3bdA"). However, inserting fewer than 2 characters does not produce a palindrome.
输入

Your program is to read from standard input. The input data has multiple test cases. In each test case, there are two lines. The first line contains one integer: the length of the input string N, 3 <= N <= 5000. The second line contains one string with length N. The string is formed from uppercase letters from 'A' to 'Z', lowercase letters from 'a' to 'z' and digits from '0' to '9'. Uppercase and lowercase letters are to be considered distinct.

输出

Your program is to write to standard output. For each test case, output one line contains one integer, which is the desired minimal number.

样例输入

5
Ab3bd

样例输出

2

```
题意：给你一串字符串，问你最少需要插入几个字符使得新字符串成为回文字符串；

思路：原字符串p，求得翻转后字符串q，得到p和q的最长公共子串后，最终答案就是p的长度-最长公共子串长度。

注意：因为N较大，开二维数组会mle。所以可以使用滚动数组对LCS的dp数组优化。

```



参考代码:

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
const int N=1e4+5;
const int M=1e7;

int dp[2][5555];
int main()
{
	io;
	 int n;
	 string p,q;
	 while(cin>>n)
	 {
	 	cin>>p;
	 	q=string(p.rbegin(),p.rend());
	 	p=" "+p;
	 	q=" "+q;
	 	int t=0;
	 	memset(dp,0,sizeof dp);
	 	for(int i=1;i<=n;i++)
	 	{
	 		t=1-t;
	 		for(int j=1;j<=n;j++)
	 		{
	 			if(p[i]==q[j])
				{
				 	dp[t][j]=max(dp[t][j],dp[1-t][j-1]+1);
				}
				else dp[t][j]=max(dp[1-t][j],dp[t][j-1]);
			}
		 }
		 cout<<n-dp[t][n]<<"\n";
	 }
}
```



 

