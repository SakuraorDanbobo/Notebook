### Question:
A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

-   The left subtree of a node contains only nodes with keys less than or equal to the node's key.
-   The right subtree of a node contains only nodes with keys greater than the node's key.
-   Both the left and right subtrees must also be binary search trees.

Insert a sequence of numbers into an initially empty binary search tree. Then you are supposed to count the total number of nodes in the lowest 2 levels of the resulting tree.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤1000) which is the size of the input sequence. Then given in the next line are the N integers in [−1000,1000] which are supposed to be inserted into an initially empty binary search tree.

### Output Specification:

For each case, print in one line the numbers of nodes in the lowest 2 levels of the resulting tree in the format:

```
n1 + n2 = n
```

where `n1` is the number of nodes in the lowest level, `n2` is that of the level above, and `n` is the sum.

### Sample Input:

```in
10
25 30 42 16 20 20 35 -5 28 22
```

### Sample Output:

```out
3 + 4 = 7
```

### 题意：
```in
给你一颗二叉搜索树，求最后两层的节点个数，a和b，最后输出两层个数和: a + b =c.
```
### 思路:
```in
1.BST建树
2.层序遍历，定义2个变量去存储2层的节点数 或者 开个数组，DFS统计每层节点数。最后输出即可。
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
const int N = 1e4+5;
const int M = 1e5;
const int mod = 1e9 + 7;
struct Node
{
	int data,pos;
	Node *l,*r;
};
void insert(Node* &rt,int x,int dep)
{
	if(rt==NULL)
	{
		rt=new Node();
		rt->pos=dep;
		rt->data=x;
		rt->l=rt->r=NULL;
		return;
	}
	if(x<=rt->data)insert(rt->l,x,dep+1);
	else insert(rt->r,x,dep+1);
}
int n;
int main()
{
	// io;
	Node *rt=NULL;
	cin>>n;
	while(n--)
	{
		int x;
		cin>>x;
		insert(rt,x,1);
	}

	queue<Node*>heap;
	heap.push(rt);
	int a=-1,b=0,cur=0;
	while(heap.size())
	{
		auto tmp=heap.front();
		heap.pop();
		if(cur!=tmp->pos)
		{
			cur=tmp->pos;
			a=b;
			b=1;
		}
		else b++;

		if(tmp->l!=NULL)heap.push(tmp->l);
		if(tmp->r!=NULL)heap.push(tmp->r);
	}
	printf("%d + %d = %d",b,a,a+b);
	// system("pause");
	return 0;
}
/*

*/

```