A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.

If we swap the left and right subtrees of every node, then the resulting tree is called the **Mirror Image** of a BST.

Now given a sequence of integer keys, you are supposed to tell if it is the preorder traversal sequence of a BST or the mirror image of a BST.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤1000). Then *N* integer keys are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first print in a line `YES` if the sequence is the preorder traversal sequence of a BST or the mirror image of a BST, or `NO` if not. Then if the answer is `YES`, print in the next line the postorder traversal sequence of that tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input 1:

```in
7
8 6 5 7 10 8 11
```

### Sample Output 1:

```out
YES
5 7 6 8 11 10 8
```

### Sample Input 2:

```in
7
8 10 11 8 6 7 5
```

### Sample Output 2:

```out
YES
11 8 10 7 5 6 8
```

### Sample Input 3:

```in
7
8 6 8 5 10 9 11
```

### Sample Output 3:

```out
NO
```



1.构造匹配

```java

import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.StreamTokenizer;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Scanner;
import java.util.Set;
import java.util.Stack;
import java.util.TreeSet;

public class Main 
{
	static Scanner sz=new Scanner(new BufferedInputStream(System.in));
	static StreamTokenizer in=new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in),32768));
	
	static final long N=(long) (1e18);
	static List<Integer>l1,l2,l3; 
	static boolean ok;
	public static void main(String[] args) 
	{ 
		 int n=sz.nextInt();
		 l1=new ArrayList<>();
		 //存储先序遍历结果
		 l2=new ArrayList<>();
		 //存储翻转后的先序遍历结果
		 l3=new ArrayList<>();
		 Node tree=null;
		 for(int i=0;i<n;i++)
		 {
			 int x=sz.nextInt();
			 l1.add(x);
			 tree=create(tree,x);
		 }
//		 overprint(tree);
		 ok=true;
		 preorder(tree);
		 MBSTpreorder(tree);
//		 prelistprint(l2);
//		 prelistprint(l3);
//		 System.out.println("---------");
//		 overprint(tree1);
//		 overprint(tree);
		 if(l1.equals(l2))
		 {
			 System.out.println("YES");
			 lastorder(tree);
		 }
		 else if(l1.equals(l3))
		 {
			 System.out.println("YES");
			 fz(tree);
			 lastorder(tree);
		 }
		 else 
		 {
			 System.out.println("NO");
		 }
		 
	}
	
	//翻转
	private static void fz(Node tree) 
	{
		if(tree==null)return;
		Node c=tree.l;
		tree.l=tree.r;
		tree.r=c;
		fz(tree.l);
		fz(tree.r);
	}

	//后序遍历
	private static void lastorder(Node tree) 
	{
		if(tree==null)return;
		lastorder(tree.l);
		lastorder(tree.r);
		if(ok)
		{
			System.out.print(tree.data);
			ok=false;
		}
		else System.out.print(" "+tree.data);
	}
	//判断2者前序是否相同
	private static boolean pd(List<Integer> l)
	{
		for(int i=0;i<l.size();i++)
		{
			if(l.get(i)!=l1.get(i))return false;
		}
		return true;
	}

	//先序遍历
	private static void preorder(Node tree)
	{
		if(tree==null)return;
		l2.add(tree.data);
		preorder(tree.l);
		preorder(tree.r);
	}
	
	//镜像先序遍历 根右左
	private static void MBSTpreorder(Node tree)
	{
		if(tree==null)return;
		l3.add(tree.data);
		MBSTpreorder(tree.r);
		MBSTpreorder(tree.l);
	}
	
	//根据先序构造二叉搜索树
	private static Node create(Node tree, int x) 
	{
		if(tree==null)
		{
			return new Node(x);
		}
		if(x<tree.data)tree.l=create(tree.l, x);
		else tree.r=create(tree.r, x);
		return tree;
	}
	static void overprint(Node t)
	{
		Queue<Node> q=new LinkedList<>();
		q.add(t);
		while(!q.isEmpty())
		{
			Node c=q.poll();
			System.out.print(c.data+" ");
			if(c.l!=null)q.add(c.l);
			if(c.r!=null)q.add(c.r);
		}
		System.out.println();
	}
	static void prelistprint(List<Integer> l)
	{
		for(Integer i:l)System.out.print(i+" ");
		System.out.println();
	}
	 
}
class Node
{
	int data;
	Node l,r;
	public Node() {};
	public Node(int x) {
		this.data=x;
		l=null;
		r=null;
	}; 
}
```



2.根据二叉搜树性质求解

假设它是二叉搜索树，一开始isMirror为false，根据二叉搜索树的性质将已知的前序转换为后序，转换过程中，如果发现最后输出的后序数组长度不为n，那就设isMirror为true，然后清空后序数组，重新再转换一次（根据镜面二叉搜索树的性质），如果依旧转换后数组大小不等于n，就输出NO否则输出YES

```java

import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.StreamTokenizer;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Scanner;
import java.util.Set;
import java.util.Stack;
import java.util.TreeSet;

public class Main 
{
	static Scanner sz=new Scanner(new BufferedInputStream(System.in));
	static StreamTokenizer in=new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in),32768));
	
	static final long N=(long) (1e18);
	static List<Integer>l1,l2,l3; 
	static boolean isfz;
	public static void main(String[] args) 
	{ 
		 int n=sz.nextInt();
		 l1=new ArrayList<>();
		 l2=new ArrayList<>();
		 for(int i=0;i<n;i++)
		 {
			 int x=sz.nextInt();
			 l1.add(x);
		 }
		 getpost(0,n-1);
		 if(l2.size()!=n)
		 {
			 isfz=true;
			 l2.clear();
			 getpost(0, n-1);
		 }
		 if(l2.size()==n)
		 {
			 System.out.println("YES");
			 for(int i=0;i<l2.size();i++)
			 {
				 if(i!=0)System.out.print(" ");
				 System.out.print(l2.get(i));
			 }
		 }
		 else System.out.println("NO");
	}
	/*
		1.bst前序就是根节点，然后逐渐变小，直到右节点逐渐变大
	bst后序算法就是由小变大直到根，然后又由大变小
		2.bst的镜像的前序就是根节点，然后逐渐变大直到右节点逐渐变小
	后序算法就是由大变小直到根，然后又由小变大
	 **/
	private static void getpost(int i, int j) 
	{
		// TODO Auto-generated method stub
		if(i>j)return ;
		
		int l=i+1,r=j;
		//判断是不是翻转后的二叉搜索树
		if(!isfz)
		{
			//从左查找比根结点值要小的
			while(l<=j && l1.get(i)>l1.get(l))l++;
			//从右查找比根结点值要大于或等于的
			while(r>i && l1.get(i)<=l1.get(r))r--;
		}
		else //翻转后的话，相反的顺序
		{
			while(l<=j && l1.get(i)<=l1.get(l))l++;
			while(r>i && l1.get(i)>l1.get(r))r--;
		}
		//不相邻，则代表不是一颗二叉搜索树
		if(l-r!=1)return;
		/*
	    1、首先构造镜像的右子数，也就是从小到根
	    去掉根节点，那么所有比根节点小的树这是左子树，重新进行判断
	    如果是bst，那么最终root+1>tail退出
	    最小的值就是root，此时root=tail
	    同样右子树也直接退出，把最小值输入v2
	    2、输入根节点
	    3、最后构造镜像的左子树，也就是从根到大，此时root+1永远大于j了
	    全部是直接判断右子数，直到找到最大的退出
	    */
		getpost(i+1, r);
		getpost(l, j);
		l2.add(l1.get(i));
	}
	 
	 
}
 
```

