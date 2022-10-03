**描述**

​	Order is an important concept in mathematics and in computer science. For example, Zorn's Lemma states: ``a partially ordered set in which every chain has an upper bound contains a maximal element.'' Order is also important in reasoning about the fix-point semantics of programs. 
​	This problem involves neither Zorn's Lemma nor fix-point semantics, but does involve order. 
Given a list of variable constraints of the form x < y, you are to write a program that prints all orderings of the variables that are consistent with the constraints. 
​	For example, given the constraints x < y and x < z there are two orderings of the variables x, y, and z that are consistent with these constraints: x y z and x z y. 



**输入**

The input consists of a sequence of constraint specifications. A specification consists of two lines: a list of variables on one line followed by a list of contraints on the next line. A constraint is given by a pair of variables, where x y indicates that x < y. 
All variables are single character, lower-case letters. There will be at least two variables, and no more than 20 variables in a specification. There will be at least one constraint, and no more than 50 constraints in a specification. There will be at least one, and no more than 300 orderings consistent with the contraints in a specification. 
Input is terminated by end-of-file. 

**输出**

For each constraint specification, all orderings consistent with the constraints should be printed. Orderings are printed in lexicographical (alphabetical) order, one per line. Output for different constraint specifications is separated by a blank line. 

**样例输入**

a b f g
a b b f
v w x y z
v y x v z v w v

**样例输出**

abfg
abgf
agbf
gabf

wxzvy
wzxvy
xwzvy
xzwvy
zwxvy
zxwvy

```c++
#include <iostream>
#include <stdio.h> 
#include <algorithm>
#include <string.h>
#include <string>
#include <vector>
#include <queue>
#define LL long long
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
using namespace std;
const int MOD = 1e7+7;
const int INF = 0x3f3f3f3f;
const int MAXN = 1e7+5;
const int N = 30;
string str;
int cnt; //输入总个数 
int vis[N];  //存储输入的数 
int m[N][N]; //邻接矩阵存储信息 
int d[N]; //入度 
char ans[N]; //存储结果 
//  拓扑 + DFS的一个结构 
void Top_sort(int x)
{
	if(x==cnt)
	{
		ans[x]='\0';
		cout<<ans<<endl;
		return;
	}
	for(int i=0;i<26;i++)
	{
//		遍历入度为0且未被访问的字母 
		if(!d[i] && vis[i])
		{
//			值为-1，处理为访问过 
			d[i]--;
//			遍历i点接下去的点 
			for(int j=0;j<26;j++)
			{
				if(m[i][j])
				{
					d[j]--;
				}
			}
//			存储结果 
			ans[x]='a'+i;
//			遍历下一个点
			Top_sort(x+1);
			
//			回溯 
			d[i]++;
			for(int j=0;j<26;j++)
			{
				if(m[i][j])
				{
					d[j]++;
				}
			}
		}
	}
}
int main()
{
	while(getline(cin,str))
	{
		cnt=0;
		memset(vis,0,sizeof(vis));
		memset(m,0,sizeof(m));
		memset(d,0,sizeof(d));
		for(int i=0;i<str.length();i++)
		{
			if(str[i]!=' ')
			{
//				记录数
				vis[str[i]-'a']=1;
				cnt++;
			}
		}
		
		getline(cin,str);
		for(int i=0;i<str.length();i+=4)
		{
			int l=str[i]-'a';
			int r=str[i+2]-'a';
			
//			邻接矩阵存图信息 
			if(!m[l][r])
			{
				m[l][r]=1;
			}
//			入度+1 
			d[r]++;
		}
		Top_sort(0);
		cout<<endl; 
	}	
}
```

