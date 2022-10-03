### Question
There is a kind of balanced binary search tree named **red-black tree** in the data structure. It has the following 5 properties:

-   (1) Every node is either red or black.
-   (2) The root is black.
-   (3) Every leaf (NULL) is black.
-   (4) If a node is red, then both its children are black.
-   (5) For each node, all simple paths from the node to descendant leaves contain the same number of black nodes.

For example, the tree in Figure 1 is a red-black tree, while the ones in Figure 2 and 3 are not.
![[Pasted image 20220529170013.png]]
Figure 1
![[Pasted image 20220529170024.png]]
Figure 2
For each given binary search tree, you are supposed to tell if it is a legal red-black tree.

### Input Specification:

Each input file contains several test cases. The first line gives a positive integer K (≤30) which is the total number of cases. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the preorder traversal sequence of the tree. While all the keys in a tree are positive integers, we use negative signs to represent red nodes. All the numbers in a line are separated by a space. The sample input cases correspond to the trees shown in Figure 1, 2 and 3.

### Output Specification:

For each test case, print in a line "Yes" if the given tree is a red-black tree, or "No" if not.

### Sample Input:

```in
3
9
7 -2 1 5 -4 -11 8 14 -15
9
11 -2 1 -7 5 -4 8 14 -15
8
10 -7 5 -6 8 15 -11 17
```

### Sample Output:

```out
Yes
No
No
```

### 题目大意：
```in
给一棵二叉搜索树的前序遍历，判断它是否为红黑树，是输出Yes，否则输出No。
红黑树有以下特征：
1.  每个节点不是红色就是黑色
2.  根节点必须是黑色
3.  定义叶子NULL节点为黑色（这里是指为空的节点而不是叶子节点）
4.  红色节点的孩子必须都是黑色
5.  从任意一点出发，到每一个叶子，途经黑色节点的数量必须均相同
红黑树本身也是一颗二叉搜索树
```
### 思路：
```in
判断主要有以下3点：
1.根结点是否为黑色
2.如果一个结点是红色，它的孩子节点是否都为黑色
3.从任意结点到叶子结点的路径中，黑色结点的个数是否相同

所以解决途径分为以下4步：
0. 根据二叉搜索树的建树规则，先序建立一棵树。
1. 判断根结点是否是黑色
2. 根据建立的树，从根结点开始遍历，如果当前结点是红色，判断它的孩子节点是否为黑色，递归返回结果
3. 从根节点开始，递归遍历，检查每个结点的左子树的高度和右子树的高度（这里的高度指黑色结点的个数），比较左右孩子高度是否相等，递归返回结果
```

### 参考代码：
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
const int N = 5e4 + 5;
const int M = 2e5+5;
const int mod = 1e9 + 7;
struct Node
{
	int data;
	Node *l,*r;
};

//同样是以BST的建树方式来建树
void create(Node* &tr,int x)
{
	if(tr==NULL)
	{
		tr=new Node();
		tr->data=x;
		tr->l=tr->r=NULL;
		return;
	}
	if(abs(x)<=abs(tr->data))create(tr->l,x);
	else create(tr->r,x);
}
//条件4判断：若当前节点为红，则2个子节点要为黑
bool pd1(Node* tr)
{
	if(tr==NULL)return true;
	//若当前节点为红，对左右节点进行判断
	if(tr->data<0)
	{
		if(tr->l!=NULL && tr->l->data<0)return false;
		if(tr->r!=NULL && tr->r->data<0)return false;
	}
	return pd1(tr->l) && pd1(tr->r);
}
//
int get(Node* tr)
{
	if(tr==NULL)return 0;
	int l=get(tr->l);
	int r=get(tr->r);
	return tr->data>0?max(l,r)+1:max(l,r);
}
//条件5判断:从任意结点到叶子结点的路径中，黑色结点的个数是否相同
bool pd2(Node* tr)
{
	if(tr==NULL)return true;
	int l=get(tr->l);
	int r=get(tr->r);
	if(l!=r)return false;
	return pd2(tr->l)&&pd2(tr->r);
}
int main()
{
	// io;
	int t,n;
	cin>>t;
	while(t--)
	{
		cin>>n;
		Node *root=NULL;
		//判断根节点是否为黑
		bool flag=1;
		for(int i=1,x;i<=n;i++)
		{
			cin>>x;
			create(root,x);
			if(i==1 && x<0)flag=0;
		}
		if(!flag || !pd1(root) || !pd2(root))cout<<"No\n";
		else cout<<"Yes\n";
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