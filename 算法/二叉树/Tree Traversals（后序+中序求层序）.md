### Question:
Suppose that all the keys in a binary tree are distinct positive integers. Given the postorder and inorder traversal sequences, you are supposed to output the level order traversal sequence of the corresponding binary tree.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the postorder sequence and the third line gives the inorder sequence. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the level order traversal sequence of the corresponding binary tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
7
2 3 1 5 7 6 4
1 2 3 4 5 6 7
```

### Sample Output:

```out
4 1 6 3 5 7 2
```

### 题意
给你一颗树的后序和中序，求层序遍历。


### 思路：
方法一：无需构造树，用map来取代。
首先，根据后序定根，中序定左右的关系来递归得到这棵树的先序遍历情况。
其次，只需要在先序遍历的过程中加一个变量index，表示当前的根结点在二叉树中所对应的下标（根节点从0开始，那么左子树表示为2*i+1,右子树表示为2*i+2），所以进行一次输出先序的递归过程中，就可以把根结点下标index及所对应的值存储在map<int, int> lev中，而又因为map是有序的，会根据index从小到大自动排序，这样递归完成后level中的值就是层序遍历的顺序。详见参考代码一。

### 参考代码一：

```c++
#include <bits/stdc++.h>

#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)

#define LL long long

#define x first

#define y second

#define PII pair<int,int>

#define PDD pair<double,double>

using namespace std;

const int INF=0x3f3f3f3f;

const int N=1e5+5;

const int M=1e7;

const int mod=1e9+7;

int n,first;

int pos[33],in[33];

map<int,int> lev;

  

void dfs(int pr,int il,int ir,int index)

{

    if(il>ir)return;

    lev[index]=pos[pr];

    int u=il;

    while(u<=ir && pos[pr]!=in[u])u++;

    //左树

    dfs(pr-1-(ir-u),il,u-1,index*2+1);

    //右树

    dfs(pr-1,u+1,ir,index*2+2);

}

int main()

{

    // io;

    cin>>n;

    first=1;

    for(int i=0;i<n;i++)

    {

        cin>>pos[i];

    }

    for(int i=0;i<n;i++)

    {

        cin>>in[i];

    }

    dfs(n-1,0,n-1,0);

    auto it=lev.begin();

    cout<<it->y;

    while(++it!=lev.end())

    {

        cout<<" "<<it->y;

    }

    system("pause");

    return 0;

}

/*

*/

```