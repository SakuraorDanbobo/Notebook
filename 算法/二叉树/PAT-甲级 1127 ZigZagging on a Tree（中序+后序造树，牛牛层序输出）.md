### Question：
Suppose that all the keys in a binary tree are distinct positive integers. A unique binary tree can be determined by a given pair of postorder and inorder traversal sequences. And it is a simple standard routine to print the numbers in level-order. However, if you think the problem is too simple, then you are too naive. This time you are supposed to print the numbers in "zigzagging order" -- that is, starting from the root, print the numbers level-by-level, alternating between left to right and right to left. For example, for the following tree you must output: 1 11 5 8 17 12 20 15.
![[Pasted image 20220530165415.png]]
### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the inorder sequence and the third line gives the postorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the zigzagging sequence of the tree in a line. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
8
12 11 20 17 1 15 8 5
12 20 17 11 15 8 5 1
```

### Sample Output:

```out
1 11 5 8 17 12 20 15
```

### 题意:
```in
给出一个树的中序和后序遍历结果，求它的蛇形层序遍历，也就是奇数层从右往左，偶数层从左往右遍历
```

### 思路：
```in
1.建树，直接使用map存储，因为map能自动根据key值自动排序。
2.层序遍历的时候，用vector存储每层的节点值。
3.遍历输出，奇数层从右往左，偶数层从左往右遍历。
```
### 参考代码：
```c++#include <bits/stdc++.h>
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
const int N = 1e5 + 5;
const int M = 2e5+5;
const int mod = 1e9 + 7;

int n;
int in[66],po[66];
map<int,int>v;
void dfs(int pr,int il,int ir,int rt)
{
	if(il>ir)return;
	v[rt]=po[pr];
	int u=il;
	while(in[u]!=po[pr])u++;

	dfs(pr-(ir-u+1),il,u-1,rt*2);
	dfs(pr-1,u+1,ir,rt*2+1);

}
int main()
{
	// io;
	cin>>n;
	for(int i=1;i<=n;i++)cin>>in[i];
	for(int i=1;i<=n;i++)cin>>po[i];
	dfs(n,1,n,1);
	vector<int>f[33];
	queue<PII>q;
	q.push({1,1});
	int len=0;
	while(q.size())
	{
		auto tmp=q.front();
		q.pop();
		int pos=tmp.first;
		len=pos;
		int rt=tmp.second;
		f[pos].push_back(v[rt]);
		if(v[rt*2]!=0)q.push({pos+1,rt*2});
		if(v[rt*2+1]!=0)q.push({pos+1,rt*2+1});
	}
 
	cout<<v[1];
	for(int i=2;i<=len;i++)
	{
		if(i%2==0)//从左往右
		{
			for(auto u:f[i])
			{
				cout<<" "<<u;
			}
		}
		else
		{
			for(int j=f[i].size()-1;j>=0;j--)
			{
				cout<<" "<<f[i][j];
			}
		}
	}
	// system("pause");
	return 0;
}
/*
5
2 2 3
2 1 5
1 4
1 3
0
111
*/


```