### Question:
A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

-   The left subtree of a node contains only nodes with keys less than the node's key.
-   The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
-   Both the left and right subtrees must also be binary search trees.

Given the structure of a binary tree and a sequence of distinct integer keys, there is only one way to fill these keys into the tree so that the resulting tree satisfies the definition of a BST. You are supposed to output the level order traversal sequence of that tree. The sample is illustrated by Figure 1 and 2.
![[Pasted image 20220604210534.png]]
### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤100) which is the total number of nodes in the tree. The next N lines each contains the left and the right children of a node in the format `left_index right_index`, provided that the nodes are numbered from 0 to N−1, and 0 is always the root. If one child is missing, then −1 will represent the NULL child pointer. Finally N distinct integer keys are given in the last line.

### Output Specification:

For each test case, print in one line the level order traversal sequence of that tree. All the numbers must be separated by a space, with no extra space at the end of the line.

### Sample Input:

```in
9
1 6
2 3
-1 -1
-1 4
5 -1
-1 -1
7 -1
-1 8
-1 -1
73 45 11 58 82 25 67 38 42
```

### Sample Output:

```out
58 25 82 11 38 67 45 73 42
```


### 题意:
```in
给出一棵二叉搜索树的每个结点的左右孩子，且已知根结点为0，给出一组应该插入这个二叉搜索树的数值，求这棵二叉树的层序遍历。
```
### 思路:
```in
首先，二叉搜索树的中序遍历就是节点从小到大的一组数。因此我们直接模拟中序遍历存储递归过程中的层数和数值。最后对层数和数值排序(因为二叉搜树中左孩子小于等于根，右孩子大于根，则每一层的节点时从小到大的)，按顺序输出即可。
```

### 参考代码:
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
const int N = 100;
const int M = 1e5;
const int mod = 1e9 + 7;
 
int f[N],n,pos;
int l[N],r[N];
vector<PII>ans; //第一个参数是层数，第二个参数是值,因为二叉搜索树从左到右是递增的
void dfs(int u,int deep)
{
	if(u==-1)return;
	dfs(l[u],deep+1);
	ans.push_back({deep,f[pos++]});
	dfs(r[u],deep+1);

}
int main()
{
	// io;
	int a,b,x;
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cin>>a>>b;
		l[i]=a;
		r[i]=b;
	}
	for(int i=0;i<n;i++)
	{
		cin>>f[i];
	}
	sort(f,f+n);
	dfs(0,1);
	sort(ans.begin(),ans.end());
	bool flag=1;
	for(auto u:ans)
	{
		if(flag)
		{
			cout<<u.second;
			flag=0;
		}
		else cout<<" "<<u.second;
	}
	
	// system("pause");
	return 0;
}
/*

*/


```