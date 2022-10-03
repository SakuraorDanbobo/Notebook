**Description**

Farmer John and Betsy are playing a game with N (1 <= N <= 30,000)identical cubes labeled 1 through N. They start with N stacks, each containing a single cube. Farmer John asks Betsy to perform P (1<= P <= 100,000) operation. There are two types of operations:
moves and counts.

* In a move operation, Farmer John asks Bessie to move the stack containing cube X on top of the stack containing cube Y.
* In a count operation, Farmer John asks Bessie to count the number of cubes on the stack with cube X that are under the cube X and report that value.

Write a program that can verify the results of the game.

**Input**

Line 1: A single integer, P

Lines 2..P+1: Each of these lines describes a legal operation. Line 2 describes the first operation, etc. Each line begins with a ‘M’ for a move operation or a ‘C’ for a count operation. For move operations, the line also contains two integers: X and Y.For count operations, the line also contains a single integer: X.

Note that the value for N does not appear in the input file. No move operation will request a move a stack onto itself.

**Output**

Print the output from each of the count operations in the same order as the input file.

**Sample Input**

6
M 1 6
C 1
M 2 4
M 2 6
C 3
C 4

**Sample Output**

1
0
2

大意:堆叠箱子，含有第i号的这堆箱子放在含有第j号的这堆箱子上面，求第i个箱子下的箱子个数。

```c++
#include <bits/stdc++.h>
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define LL long long
using namespace std;
const int MOD = 1e7+7;
const int INF = 0x3f3f3f3f;
const int MAXN = 1e7+5;
const int N=1e5+2;
int pre[N]; //父节点 
int dis[N]; //i个箱子下的箱子总个数 
int sum[N]; //i个箱子堆的箱子总个数 
int find(int x)
{
	if(x!=pre[x])
	{
//		父节点状态没更新，因此错误 (自下而上了) 
//		dis[x]+=dis[pre[x]];
		int fa=pre[x];
		pre[x]=find(pre[x]); //应该自上而下 ，更新完根节点后再跟新掉子节点 
		//更新个数,加上父节点节点下箱子的总个数 
		dis[x]+=dis[fa];
	}
	return pre[x]; 
}
void join(int a,int b)
{
	int a1=find(a);
	int b1=find(b);
	if(a1!=b1)
	{
		pre[a1]=b1;
		//b1堆所有的立方体个数 ，即为a1点下箱子总个数 
		dis[a1]=sum[b1]; 
		//加上a1堆所有的立方体个数，即为b1堆所拥有的立方体总个数 
		sum[b1]+=sum[a1]; 
	}
}
int main()
{
	IO;
	int t,x,y;
	char a;
	cin>>t;
	for(int i=1;i<=t;i++)
	{
		pre[i]=i;
		dis[i]=0;
		sum[i]=1;
	}
	while(t--)
	{
		cin>>a;
		//合并 
		if(a=='M')
		{
			cin>>x>>y;
			join(x,y);
		}
		//查询 
		else
		{
			cin>>x;
//			当多堆箱子移动到其他堆箱子上时,箱子上的子节点高度并没有更新，
//			因此需要通过找寻父节点来更新下节点值 
			find(x); 
			cout<<dis[x]<<"\n";
		}
	}
}
/*
4
M 1 2
M 3 4
M 2 3
C 1

*/

```

