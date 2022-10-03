### Question
The lowest common ancestor (LCA) of two nodes U and V in a tree is the deepest node that has both U and V as descendants.
A binary search tree (BST) is recursively defined as a binary tree which has the following properties:
The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
Given any two nodes in a BST, you are supposed to find their LCA.

### Input Specification:
Each input file contains one test case. For each case, the first line gives two positive integers: M (≤ 1,000), the number of pairs of nodes to be tested; and N (≤ 10,000), the number of keys in the BST, respectively. In the second line, N distinct integers are given as the preorder traversal sequence of the BST. Then M lines follow, each contains a pair of integer keys U and V. All the keys are in the range of int.

### Output Specification:
For each given pair of U and V, print in a line LCA of U and V is A. if the LCA is found and A is the key. But if A is one of U and V, print X is an ancestor of Y. where X is A and Y is the other node. If U or V is not found in the BST, print in a line ERROR: U is not found. or ERROR: V is not found. or ERROR: U and V are not found..

### Sample Input:
6 8
6 3 1 2 5 4 8 7
2 5
8 7
1 9
12 -3
0 8
99 99
### Sample Output:
LCA of 2 and 5 is 3.
8 is an ancestor of 7.
ERROR: 9 is not found.
ERROR: 12 and -3 are not found.
ERROR: 0 is not found.
ERROR: 99 and 99 are not found.

### 题意：
```in
给你一颗前序二叉搜索树，给定2个节点，问他们的最近公共祖先是谁？
```
### 思路：
```in
首先我们使用map来标记树中所有出现过的结点，
接着在查询u，v节点的时候，遍历一遍pre数组，将当前结点标记为t，如果u和v分别在 t 的左、
右，或者u、v其中一个就是当前a，说明找到了这个共同最低祖先a，退出当前循环，最后根据要求输出结果即可.
```
### 参考代码:
```c++
#include <bits/stdc++.h>

using namespace std;

int pre[11111];
map<int,bool> vis;
int main()
{
    int m,n;
    cin>>m>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>pre[i];
        vis[pre[i]]=1;
    }
    while(m--)
    {
        int u,v,t;
        cin>>u>>v;
        for(int i=1;i<=n;i++)
        {
            t=pre[i];
            //若等于其中一个节点了，则该点为LCA
            if(t==u || t==v)break;
            //当满足当前节点t , (u t v)或者 (v t u),则t点为LCA
            if((t>u && t<v) || (t>v && t<u))break;
        }
        if(!vis[u] && !vis[v])
        {
            printf("ERROR: %d and %d are not found.\n",u,v);
        }
        else if(!vis[u])
        {
            printf("ERROR: %d is not found.\n",u);
        }
        else if(!vis[v])
        {
            printf("ERROR: %d is not found.\n",v);
        }
        else if(t==u || t==v)
        {
            printf("%d is an ancestor of %d.\n",t,t==u?v:u);
        }
        else
        {
            printf("LCA of %d and %d is %d.\n",u,v,t);
        }
    }
}
```