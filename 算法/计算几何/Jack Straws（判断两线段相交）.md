#### 描述

In the game of Jack Straws, a number of plastic or wooden "straws" are dumped on the table and players try to remove them one-by-one without disturbing the other straws. Here, we are only concerned with if various pairs of straws are connected by a path of touching straws. You will be given a list of the endpoints for some straws (as if they were dumped on a large piece of graph paper) and then will be asked if various pairs of straws are connected. Note that touching is connecting, but also two straws can be connected indirectly via other connected straws.

#### 输入

Input consist multiple case,each case consists of multiple lines. The first line will be an integer n (1 < n < 13) giving the number of straws on the table. Each of the next n lines contain 4 positive integers,x1,y1,x2 and y2, giving the coordinates, (x1,y1),(x2,y2) of the endpoints of a single straw. All coordinates will be less than 100. (Note that the straws will be of varying lengths.) The first straw entered will be known as straw #1, the second as straw #2, and so on. The remaining lines of the current case(except for the final line) will each contain two positive integers, a and b, both between 1 and n, inclusive. You are to determine if straw a can be connected to straw b. When a = 0 = b, the current case is terminated.

When n=0,the input is terminated.

There will be no illegal input and there are no zero-length straws.

#### 输出

You should generate a line of output for each line containing a pair a and b, except the final line where a = 0 = b. The line should say simply "CONNECTED", if straw a is connected to straw b, or "NOT CONNECTED", if straw a is not connected to straw b. For our purposes, a straw is considered connected to itself.

#### 样例输入

7
1 6 3 3 
4 6 4 9 
4 5 6 7 
1 4 3 5 
3 5 5 5 
5 2 6 3 
5 4 7 2 
1 4 
1 6 
3 3 
6 7 
2 3 
1 3 
0 0

2
0 2 0 0
0 0 0 1
1 1
2 2
1 2
0 0

0

#### 样例输出

CONNECTED 
NOT CONNECTED 
CONNECTED 
CONNECTED 
NOT CONNECTED 
CONNECTED
CONNECTED
CONNECTED
CONNECTED

 
#### 题意:
```in
给你n条线段的两端坐标(x1,y1),(x2,y2),接着询问2条线段是否属于同一个集合，即是否有交点(可以间接，类似A∩B，B∩C，则A∩C)。
```
#### 思路：
```in
首先需要明确，如何快速求2条线段相交？这里使用快速排斥+跨立实验方法解决。
当2条线段相交时，我们直接将他们归为同一集合，否则不进行操作。
```
#### 参考代码：
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
const int N = 4e6 + 5;
const int M = 1e5;
const int mod = 1e9 + 7;

int fa[66];
PII l[66],r[66];
int find(int x){return x==fa[x]?x:fa[x]=find(fa[x]);}
void join(int a,int b){fa[find(b)]=find(a);}
bool ok(int u,int v)
{
	int xu1=l[u].first,yu1=l[u].second,xu2=r[u].first,yu2=r[u].second;
	int xv1=l[v].first,yv1=l[v].second,xv2=r[v].first,yv2=r[v].second;
	
	//判断是否相交
	if((max(xu1,xu2)>=min(xv1,xv2) && min(xu1,xu2)<=max(xv1,xv2))
	&& (max(yu1,yu2)>=min(yv1,yv2) && min(yu1,yu2)<=max(yv1,yv2)))
	{
		//判断A*B的向量鸡<0(相交)，=0(重叠)
		if(((xv1-xu1)*(yu2-yu1)-(yv1-yu1)*(xu2-xu1))*
		((xv2-xu1)*(yu2-yu1)-(yv2-yu1)*(xu2-xu1))<=0 &&
		((xu1-xv1)*(yv2-yv1)-(yu1-yv1)*(xv2-xv1))*
		((xu2-xv1)*(yv2-yv1)-(yu2-yv1)*(xv2-xv1))<=0
		)
		{
			return 1;
		}
		else return 0;
	}
	else return 0;

}
int main()
{
	// io;
	int n;
	while(cin>>n && n)
	{	
		for(int i=1;i<=n;i++)fa[i]=i;
		for(int i=1;i<=n;i++)
		{
			cin>>l[i].first>>l[i].second>>r[i].first>>r[i].second;
			for(int j=1;j<i;j++)
			{
				//求两线段有无交点
				if(ok(i,j))
				{
					join(i,j);
				}
			}
		} 


		int a,b;

		while(cin>>a>>b && (a+b))
		{
			if(find(a)==find(b))cout<<"CONNECTED\n";
			else cout<<"NOT CONNECTED\n";
		}
	}

	
	// system("pause");
	return 0;
}
/*
 
*/

```