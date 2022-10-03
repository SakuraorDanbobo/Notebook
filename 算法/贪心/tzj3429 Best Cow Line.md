**描述**

FJ is about to take his N (1 <= N <= 3,0000) cows to the annual "Farmer of the Year" competition. In this contest every farmer arranges his cows in a line and herds them past the judges.

The contest organizers adopted a new registration scheme this year:simply register the initial letter of every cow in the order they will appear (i.e., If FJ takes Bessie, Sylvia, and Dora in that order he just registers BSD). After the registration phase ends,
every group is judged in increasing lexicographic order according to the string of the initials of the cows' names.

FJ is very busy this year and has to hurry back to his farm, so he wants to be judged as early as possible. He decides to rearrange his cows, who have already lined up, before registering them.

FJ marks a location for a new line of the competing cows. He then proceeds to marshal the cows from the old line to the new one by repeatedly sending either the first or last cow in the (remainder of the) original line to the end of the new line. When he's finished,FJ takes his cows for registration in this new order.

Given the initial order of his cows, determine the least lexicographic string of initials he can make this way.

**输入**

\* Line 1: A single integer: N

\* Lines 2..N+1: Line i+1 contains a single initial ('A'..'Z') of the cow in the ith position in the original line

**输出**

The least lexicographic string he can make. Every line (except perhaps the last one) contains the initials of 80 cows ('A'..'Z') in the new line.

**样例输入**

6
A
C
D
B
C
B

**样例输出**

ABCBCD

**提示**

OUTPUT DETAILS:

Step  Original   New
\#1   ACDBCB
\#2   CDBCB   	A
\#3   CDBC  	   AB
\#4   CDB    	   ABC
\#5   CD    	     ABCB
\#6   D 		      ABCBC
\#7 				   ABCBCD

 题意：给你一个字符串str，每次从左或者从右取字符，问重构后最小字典序的字符串。

参考代码：

```c++
#include <bits/stdc++.h>
#define x first
#define y second
#define LL long long
#define PII pair<int,int>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6;
int main()
{
    int n,len;
    cin>>n;
    char f[n];
    for(len=0;len<n;len++)cin>>f[len];
    
    int a=0,b=len-1,x=0;
    
    while(a<=b)
    {
    	//取左还是取右判断 
    	bool left=0; 
    	//左串与右串比较 
    	for(int i=0;a+i<=b-i;i++)
    	{
    		if(f[a+i]<f[b-i])  //左串小于右串 
    		{
    			left=1;
    			break;
			}
			else if(f[a+i]>f[b-i])break; //右串小于左串
			
			//相等时继续比较下一个字符大小 
		}
		
		if(left)cout<<f[a++]; //取左 
		else cout<<f[b--];   //取右 
		x++;
		if(x==80)
		{
			x=0;
			cout<<"\n";	
		} 	
	}
    return 0;
}
```

