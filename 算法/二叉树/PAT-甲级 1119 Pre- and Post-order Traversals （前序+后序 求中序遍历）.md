### Question:
Suppose that all the keys in a binary tree are distinct positive integers. A unique binary tree can be determined by a given pair of postorder and inorder traversal sequences, or preorder and inorder traversal sequences. However, if only the postorder and preorder traversal sequences are given, the corresponding tree may no longer be unique.

Now given a pair of postorder and preorder traversal sequences, you are supposed to output the corresponding inorder traversal sequence of the tree. If the tree is not unique, simply output any one of them.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 30), the total number of nodes in the binary tree. The second line gives the preorder sequence and the third line gives the postorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first printf in a line `Yes` if the tree is unique, or `No` if not. Then print in the next line the inorder traversal sequence of the corresponding binary tree. If the solution is not unique, any answer would do. It is guaranteed that at least one solution exists. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input 1:

```in
7
1 2 3 4 6 7 5
2 6 7 4 5 3 1
```

### Sample Output 1:

```out
Yes
2 1 6 4 7 3 5
```

### Sample Input 2:

```in
4
1 2 3 4
2 4 3 1
```

### Sample Output 2:

```out
No
2 1 3 4
```

### 题意：
```in
给你前序和后序遍历，求中序的遍历。
```

### 注意点:
```in
1.测试点1如果错误的话，数组大小需要开到100左右即可。一开始开66，错了。
2.除测试点1外，全部报格式错误。这个需要在中序遍历输出完后输出一个换行即可。
```
### 思路：
```in
首先，对于前序和后序遍历的2个特点:
特点1：前序遍历，第一个节点肯定是根，后序遍历最后一个节点肯定是根
特点2：前序的根节点后面紧邻的肯定是左子树节点，后序根节点前面紧邻的肯定是右子树节点(存在只有左子树或只有右子树的情况)。

那么，我们对前序(i=[1,n-1])进行枚举,再去后序(j=[2,n])中找到对应的前根下标，然后对前根的i+1节点与后根的j-1节点进行比较,若 i+1节点值 = j-1节点值，则无法确定左右子树，标记flag=0，并且若当前左或右节点未被访问，就成为当前根节点的左或右节点，否则我们进行标记，若当前左右节点未被访问，就成为当前根节点的左右节点。(因为前序遍历是层层递进的，因此我们确定左右孩子节点的时候，需要进行标记。)

最后详见代码*
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

int pre[N],post[N];
int ll[N],rr[N],vis[N];
bool f;
void inorder(int u)
{
	if(u==-1)return;
	inorder(ll[u]);
	if(!f)
	{
		cout<<u;
		f=1;
	}
	else cout<<" "<<u;
	inorder(rr[u]);
}
int main()
{
	// io;
	int n;
	fill(ll,ll+N,-1);
	fill(rr,rr+N,-1);
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>pre[i];
	}
	for(int i=1;i<=n;i++)
	{
		cin>>post[i];
	}
	int j;
	bool only=1; //判断是否唯一

	//前序遍历，这里遍历下标[1,n-1]，留出边界，不用判断
	for(int i=1;i<n;i++)
	{
		//前序遍历的根节点
		int rt=pre[i];
		//l为前序根节点的下层根节点，l为rt的左节点
		int l=pre[i+1];

		//后序遍历，遍历下标[2,n]，同样留出边界
		for(j=2;j<=n;j++)
		{
			if(rt==post[j])break;
		}
		//r为后序遍历的下层根节点，r为rt的右节点
		int r=post[j-1];

		vis[rt]=1;
		//若2根节点相同，则无法确定左右子树
		if(l==r)
		{
			//这里还可以标记中序遍历的数量,only^2即可。
			only=0;
		 	if(!vis[l])
			{
				 rr[rt]=l;
				 vis[l]=1;
			}
		}
		else
		{
			if(!vis[l])
			{
				ll[rt]=l;
				vis[l]=1;
			}
			if(!vis[r])
			{
				rr[rt]=r;
				vis[r]=1;
			}
		}
	}
	cout<<(only==1?"Yes":"No")<<"\n";
	inorder(pre[1]);
	cout<<"\n";
	// for(int i=1;i<=n;i++)
	// {
	// 	cout<<i<<" : "<<ll[i]<<" "<<rr[i]<<"\n";
	// }
	// system("pause");
	return 0;
}
/*

*/

```