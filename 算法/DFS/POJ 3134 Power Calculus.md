Question

给定 _n_，询问至少要多少次乘或除已经计算出来的数，使得 _x_ 可以变成 _x^n_。

Input

多组数据，最后一行以 _0_ 结尾。每行包含一个整数 _n_。0 < _n_ ≤ 1000。

Output

输出一行，表示答案。

Sample Input

1
31
70
91
473
512
811
953
0

Sample Output

0
6
8
9
11
9
13
12

思路：
使用迭代加深搜索，那么什么是迭代加深搜索？
迭代加深搜索(Iterative Deepening DFS,IDDFS)是
一种结合了DFS和BFS思想的搜索方法。当搜索树很深且很宽的时候，用DFS会陷入递归无法返回，用BFS队列空间会爆炸，那么可以试试IDDFS，简单来说，就是每次限制搜索深度的DFS。比如DFS搜索k层，若没有找到可行解则立即返回，再DFS搜索k+1层，直到找到可行解为止，在层数上采用BFS思想来逐步扩大DFS的搜索深度。

IDA\*辅助剪枝
 估价函数对迭代加深搜索的优化，即乐观估计剪枝。
 当找到解需要的至少层数+当前层数>层数限制时，直接退出。
 


参考代码：
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long
#define x first
#define y second
#define PII pair<int,int>
#define PIS pair<int,string>
#define PIII pair<int,PII>
#define PDD pair<double,double>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=110;
const int M=1e7;
const int mod=1e9+7;
/*
思路： 从小到大枚举深度上限，
剪枝：（当每次取最大的两个数相加仍然小于n时要剪枝 。
因为以最快的方式增长的话其指数是按照2的幂次增加的，
所以当前序列最大的数乘以2^(maxd-d)之后仍小于n，则剪枝 。

*/
int n,depth;
int f[1111];
//当前层数 , 当前指数值 
bool dfs(int cur,int sum)
{
	//当前指数sum，剩下步数每次走最优最后也小于n，则剪枝 
	//sum^(depth-cur)
	if((sum<<depth-cur)<n)return false;
	//超过深度约束，仍搜不，剪枝 
	if(cur>depth)return false;
	//得到结果，返回true 
	if(sum==n)return true;
	
	f[cur]=sum; //记录当前层的指数值 
	//遍历之前计算过的值,枚举下一层的值 
	for(int i=0;i<=cur;i++)
	{
		//加 ,满足条件 
		if(dfs(cur+1,sum+f[i]))return true;
		//减，满足条件 
		if(dfs(cur+1,sum-f[i]))return true;
	}
	return false;
}
int main()
{
	while(cin>>n && n)
	{
		depth=0;
		memset(f,0,sizeof f);
		while(!dfs(0,1))//若不满足条件 
		{
			//清空数组 
			memset(f,0,sizeof f);
			//搜索深度+1 
			depth++;		
		}
		cout<<depth<<"\n"; 
	} 
 } 

```