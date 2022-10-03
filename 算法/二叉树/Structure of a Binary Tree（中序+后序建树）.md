#### Question
Suppose that all the keys in a binary tree are distinct positive integers. Given the postorder and inorder traversal sequences, a binary tree can be uniquely determined.

Now given a sequence of statements about the structure of the resulting tree, you are supposed to tell if they are correct or not. A statment is one of the following:

-   A is the root
-   A and B are siblings
-   A is the parent of B
-   A is the left child of B
-   A is the right child of B
-   A and B are on the same level
-   It is a full tree

Note:

-   Two nodes are **on the same level**, means that they have the same depth.
-   A **full binary tree** is a tree in which every node other than the leaves has two children.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the postorder sequence and the third line gives the inorder sequence. All the numbers in a line are no more than 103 and are separated by a space.

Then another positive integer M (≤30) is given, followed by M lines of statements. It is guaranteed that both `A` and `B` in the statements are in the tree.

### Output Specification:

For each statement, print in a line `Yes` if it is correct, or `No` if not.

### Sample Input:

```in
9
16 7 11 32 28 2 23 8 15
16 23 7 32 11 2 28 15 8
7
15 is the root
8 and 2 are siblings
32 is the parent of 11
23 is the left child of 16
28 is the right child of 2
7 and 11 are on the same level
It is a full tree
```

### Sample Output:

```out
Yes
No
Yes
No
Yes
Yes
Yes
```

### 题意
```in
这题给你树的后序遍历和中序遍历，问节点间的7种关系。
```
### 思路：
```in
老套路了，直接递归建树的过程中记录每个节点的左孩子ll，右孩子rr，父节点fa，以及判断所有非叶节点是否都有2个子节点isfull。
最后就是询问的字符串处理下。
```
### 参考代码
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PID pair<int, double>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1e3 + 5;
const int M = 1e6;
const int mod = 1e9 + 7;
const int add = 1e6;
  
int in[66],po[66];
int deep[N]; //深度
int fa[N]; //父节点
int ll[N],rr[N]; //左右节点
int ans;
bool isfull;

// 后序右边界 , 中序[il,ir] ,当前深度dep ,父节点 pre, 左孩子还是右孩子,dir: 0 , 1
void dfs(int pr,int il,int ir,int dep,int pre,int dir)
{
    if(il>ir)return;

    int root=po[pr];
    //记录当前深度
    deep[root]=dep;
    //根节点
    if(pre==-1)ans=root;
    if(dir==0) //左节点
    {
        //更新父节点
        fa[root]=pre;
        //更新左节点
        ll[pre]=root;
    }
    else if(dir==1)//右节点
    {
        //更新父节点
        fa[root]=pre;
        //更新右节点
        rr[pre]=root;
    }

    int u; 
    for(u=il;u<=ir;u++)
    {
        if(po[pr]==in[u])break;
    }

    //左子树递归
    dfs(pr-1-(ir-u),il,u-1,dep+1,root,0);
    //右子树递归
    dfs(pr-1,u+1,ir,dep+1,root,1);

    if(ll[root]==-1 && rr[root]!=-1)isfull=true;
    else if(ll[root]!=-1 && rr[root]==-1)isfull=true;
}
int main()
{
    // io;
    fill(ll,ll+N,-1);
    fill(rr,rr+N,-1);
    fill(fa,fa+N,-1);
 
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)cin>>po[i];
    for(int i=1;i<=n;i++)cin>>in[i];

    isfull=false;

    dfs(n,1,n,1,-1,-1);
    
    int k;
    string str;
    cin>>k;
    cin.ignore();
    string res[10];
    while(k--)
    {
        getline(cin,str);
        int len=str.length();
        stringstream ss;
        string s;
        ss<<str;
        int cnt=0;
        while(ss>>s)
        {
            res[cnt++]=s;
        }
        if(str.find("root")!=-1)
        {
            if(to_string(ans)==res[0])cout<<"Yes\n";
            else cout<<"No\n";
        }
        else if(str.find("siblings")!=-1 )
        {
            int l=atoi(res[0].c_str());
            int r=atoi(res[2].c_str());
            if(fa[l]==fa[r])cout<<"Yes\n";
            else cout<<"No\n";
        }
        else if(str.find("parent")!=-1)
        {
            int l=atoi(res[0].c_str());
            int r=atoi(res[5].c_str());
            if(fa[r]==l)cout<<"Yes\n";
            else cout<<"No\n";
        }
        else if(str.find("left")!=-1)
        {
            int l=atoi(res[0].c_str());
            int r=atoi(res[6].c_str());
            if(l==ll[r])cout<<"Yes\n";
            else cout<<"No\n";
        }
        else if(str.find("right")!=-1)
        {
            int l=atoi(res[0].c_str());
            int r=atoi(res[6].c_str());
            if(l==rr[r])cout<<"Yes\n";
            else cout<<"No\n";
        }
        else if(str.find("level")!=-1 )
        {
            int l=atoi(res[0].c_str());
            int r=atoi(res[2].c_str());
            if(deep[l]==deep[r])cout<<"Yes\n";
            else cout<<"No\n";
        }
        else
        {
            if(!isfull)cout<<"Yes\n";
            else cout<<"No\n";
        }
    }

    system("pause");
    return 0;
}
/*
5
11 12 7 5 4
11 7 12 4 5
7
4 is the root
11 and 12 are siblings
5 is the parent of 11
11 is the left child of 7
12 is the right child of 4
7 and 5 are on the same level
It is a full tree
*/
```