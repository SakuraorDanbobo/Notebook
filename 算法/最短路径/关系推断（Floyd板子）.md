Description
给你一些已经确定的元素之间的关系，请你判断是否能从这些元素关系中推断出其他的元素关系。

Input
输入的第一行是一个整数N，表示测试数据的组数。
每组输入首先是一个正整数m（m<=100），表示给定元素关系的个数。

接下来m行，每行一个元素关系，格式为：元素1<元素2   或者 元素1>元素2

元素用一个大写字母表示，输入中不会包含冲突的关系。

Output
对于每组输入，第一行输出：Case d:，d是测试数据的序号，从1开

接下来输出所有推断出的新的元素关系，按照字典序从小到大排序，格式为详见样例输出。

每个元素关系占一行，输入中给定的元素关系不要输出。

如果没有新的元素关系推断出来，则输出NONE。

Sample Input 
2
3
A<B
C>B
C<D
2
A<B
C<D

Sample Output
Case 1:
A<C
A<D
B<D
Case 2:
NONE
 

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <string.h>
using namespace std;
int n,m;
int t[26][26]; //已有的关系 
int vis[26][26]; //更新后的关系 
char str[3];
char a,b,c;
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		memset(vis,0,sizeof(vis));
		memset(t,0,sizeof(t));
		cin>>m;
		while(m--)
		{
			cin>>str;
			a=str[0];
			b=str[1];
			c=str[2];
			if(b=='>')
			{
				char d=a;
				a=c;
				c=d;
			}
			int l=a-'A';
			int r=c-'A';
			
			vis[l][r]=1;
			t[l][r]=1;
		} 
		
//		floyd传递关系值
		for(int k=0;k<26;k++)
		{
			for(int l=0;l<26;l++)
			{
				for(int j=0;j<26;j++)
				{
					if(vis[l][k] && vis[k][j])
					{
						vis[l][j]=1;
					}
				}
			}
		} 
		cout<<"Case "<<i<<":"<<endl; 
		int isNo=1;
		for(int k=0;k<26;k++)
		{
			for(int l=0;l<26;l++)
			{
				
				if(t[k][l])continue;
				if(vis[k][l])
				{
					isNo=0;
					char a1=(k+'A');
					char a2=(l+'A');
					cout<<a1<<"<"<<a2<<endl;
				}
			}
		}
		if(isNo)cout<<"NONE"<<endl;
	} 
	
}
```

