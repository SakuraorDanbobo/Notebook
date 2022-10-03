描述

Given a pair of positive integers, for example, 6 and 110, can this equation 6 = 110 be true? The answer is yes, if 6 is a decimal number and 110 is a binary number.

Now for any pair of positive integers N1 and N2, your task is to find the radix of one number while that of the other is given.



输入

Each input file contains one test case. Each case occupies a line which contains 4 positive integers:

N1 N2 tag radix

Here N1 and N2 each has no more than 10 digits. A digit is less than its radix and is chosen from the set { 0-9, a-z } where 0-9 represent the decimal numbers 0-9, and a-z represent the decimal numbers 10-35. The last number radix is the radix of N1 if tag is 1, or of N2 if tag is 2.



输出

For each test case, print in one line the radix of the other number so that the equation N1 = N2 is true. If the equation is impossible, print Impossible. If the solution is not unique, output the smallest possible radix.

样例输入

6 110 1 10


样例输出

2


提示

输入样例2：

1 ab 1 2

输出样例2：

Impossible



```c++
#include <bits/stdc++.h>
#define x first
#define y second
#define LL long long
#define PII pair<int,int>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6+5;

int changeval(char c)
{
	if(c>='0' && c<='9')return c-'0';
	else return (c-'a')+10;
}
int find(string str) //找到数中最大的一位数 
{
	int maxc=0;
	for(int i=0;i<str.length();i++)
	{
		maxc=max(maxc,changeval(str[i]));
	 } 
	 return maxc;
}
LL getval(string str,LL base)
{
	LL sum=0;
	for(int i=0;i<str.length();i++)
	{
		sum=sum*base+changeval(str[i]);
	}
	return sum;
}
void find(string a1,string a2,LL base)
{
	LL x=getval(a1,base);
	//枚举下界一定是 a2 中最大的一位数+1 
	 LL l=find(a2)+1;
	 //枚举上界 
	 LL r=x+1;
	 LL mid,val;
	 while(l<=r) //枚举进制数 
	 {
	 	mid=(l+r)>>1;
	 	val=getval(a2,mid);
	 	if(val==x)
	 	{
	 		cout<<mid;
	 		return;
		 }
		 //可能出现溢出，因此 val<0 特判 
		 else if(val>x || val<0)r=mid-1;
		 else l=mid+1;
	 }
	 if(l>r)cout<<"Impossible";
}
int main()
{
 	string a,b;
 	LL tag,base;
 	LL x,y;
 	cin>>a>>b>>tag>>base;
 	if(tag==1)
 	{
 		find(a,b,base);
	 }
	 else 
	 {
	 	find(b,a,base);
	 }
    return 0;
}
```

