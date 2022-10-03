#### Question
 A Cartesian tree is a binary tree constructed from a sequence of distinct numbers. The tree is heap-ordered, and an inorder traversal returns the original sequence. For example, given the sequence { 8, 15, 3, 4, 1, 5, 12, 10, 18, 6 }, the min-heap Cartesian tree is shown by the figure.
![在这里插入图片描述](https://img-blog.csdnimg.cn/d4ddc99a84ff4bda8ca141f45578def7.png)
Your job is to output the level-order traversal sequence of the min-heap Cartesian tree.

#### Input Specification:
Each input file contains one test case. Each case starts from giving a positive integer N (≤30), and then N distinct numbers in the next line, separated by a space. All the numbers are in the range of int.

#### Output Specification:
For each test case, print in a line the level-order traversal sequence of the min-heap Cartesian tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the beginning or the end of the line.

#### Sample Input:
```in
10
8 15 3 4 1 5 12 10 18 6
```
#### Sample Output:
```out
1 3 5 8 4 6 15 10 12 18
```

#### 题意:
```in
一棵笛卡尔树满足 , 值最小堆 +  下标二叉搜索树的形式，
最后层序输出
```
>关于笛卡尔树的详细内容，指路:
>https://oi-wiki.org/ds/cartesian-tree/
#### 思路：
```in
对于笛卡尔树构造,我们考虑将元素按照键值index(下标)排序。
然后一个一个插入到当前的笛卡尔树中。那么每次我们插入的
元素必然在这个树的右链(右链：即从根结点一直往右子树走，
经过的结点形成的链)的末端。于是我们执行这样一个过程，
从下往上比较右链结点与当前结点u的val[u]，如果找到了一
个右链上的结点x满足val[x]<val[u]，就把u接到x的右儿子上，
而x原本的右子树就变成u的左子树。

对于规律造树，我们每次从[l,r]取一个最小的值，以该点mid为根，
然后递归构造左子树[l,mid-1],右子树[mid+1,r]。做法应该简单的。
```

#### 参考代码：
```c++
#include <bits/stdc++.h>

using namespace std;
const int N=100;
const int INF=0x3f3f3f3f;
int n,pos;
int h[N];
int l[N],r[N];
int sta[N];
//单调栈造树 
void creat1()
{
	int top=0; //栈顶元素 
	for(int i=1;i<= n;i++)
	{
		//弹栈，找到第一个小于等于当前元素的 
		while(top && h[sta[top]]>h[i])
		{
			top--;
		}
		//栈空 
		if(!top)
		{ 
			//当前节点为根节点 
			pos=i;
			//栈顶元素为当前点的左子树
			l[i]=sta[top+1];
		}
		else //栈不为空 
		{
			//将当前点插入栈顶节点下方，那么当前节点的左子树为栈顶元素的右子树 
			l[i]=r[sta[top]];
			//当前节点为栈顶成为栈顶元素的右子树 
			r[sta[top]]=i;
		}
		//将key(下标)值入栈 
		sta[++top]=i; 
	}
}
//STL 栈模拟造树 
void creat2()
{
	stack<int> s;
	int x=0; //栈顶元素 
	for(int i=1;i<=n;i++)
	{
		while(s.size())
		{
			x=s.top();
			if(h[x]>h[i])
			{
				s.pop();
			}
			else 
			{
				l[i]=r[x];
				r[x]=i;
				break;
			}
		}
		if(s.empty())
		{
			//当前节点为根节点 
			pos=i;
			l[i]=x;
		}
		s.push(i); 
	}
}
//根据规律造树
//[l,r] ,dir是相对于父节点 pid 下标的左子树或右子树, 
void creat3(int ll, int rr,int dir,int pid)
{
	if(ll>rr)return;
	
	//找到当前区间里的最小值
	int minc=INF;
	int index=0; //得到下标值 
	for(int i=ll;i<=rr;i++) 
	{
		if(minc>h[i])
		{
			minc=h[i];
			index=i;
		}
	}
	//得到根节点 
	if(pos==0)pos=index;
	//左子树 
	if(dir==0)
	{
		l[pid]=index; 
	 } 
	 //右子树 
	 else if(dir==1)
	 {
	 	r[pid]=index;
	 }
	
	//递归左子树 
	creat3(ll,index-1,0,index);
	//递归右子树 
	creat3(index+1,rr,1,index);
}

//层序遍历 
void print()
{
	queue<int> q;
	q.push(pos);
    bool f=0;
	while(q.size())
    {
        int c=q.front();
        q.pop();
        if(f)cout<<" ";
        cout<<h[c];
        if(l[c]>0)q.push(l[c]);
        if(r[c]>0)q.push(r[c]);
        f=1;
    }
}
int main()
{
	cin>>n;
	for(int i=1;i<=n;i++)
	{
		cin>>h[i];
	} 
	fill(l,l+N,-1);
	fill(r,r+N,-1);
//	creat1();
//	creat2();
	creat3(1,n,-1,-1);
	print();
}
```