### Question
A graph which is connected and acyclic can be considered a tree. The height of the tree depends on the selected root. Now you are supposed to find the root that results in a highest tree. Such a root is called **the deepest root**.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer N (≤104) which is the number of nodes, and hence the nodes are numbered from 1 to N. Then N−1 lines follow, each describes an edge by given the two adjacent nodes' numbers.

### Output Specification:

For each test case, print each of the deepest roots in a line. If such a root is not unique, print them in increasing order of their numbers. In case that the given graph is not a tree, print `Error: K components` where `K` is the number of connected components in the graph.

### Sample Input 1:

```in
5
1 2
1 3
1 4
2 5
```

### Sample Output 1:

```out
3
4
5
```

### Sample Input 2:

```in
5
1 3
1 4
2 5
3 4
```

### Sample Output 2:

```out
Error: 2 components
```

### 题意：
```in
给你n个点，n-1条边，问从任意节点出发，哪些节点是深度最大的，(结果从小到大打印)
需要注意的是本题还要判断是否是一颗树，即有无连通。（可以用并查集或者dfs解决）
```

### 思路：
```in
1.首先用并查集解决是否为棵树
2.求点。有点类似树的直径，我们从任意节点出发都可以，先跑一遍dfs，找到最远的一些点(最远点可能不止一个)。然后再从最远点中的任意一个点开始，再跑一遍dfs，将另一端的一些最远点得到即可。
```

### 参考代码：
```in
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 2e5 + 5;
const int M = 1e5;
const int mod = 1e9 + 7;

int fa[N];
int find(int x){return x==fa[x]?x:fa[x]=find(fa[x]);}
void join(int a,int b){fa[find(b)]=find(a);}

vector<int>v[N];
set<int> s1,s2;
bool vis[N];
int root,maxp;
void dfs(int u,int pos,set<int> &st)
{
    vis[u]=1;
    if(pos>maxp)
    {
        maxp=pos;
        st.clear();
        st.insert(u);
        root=u;
    }
    else if(pos==maxp)
    {
        st.insert(u);
    }
    for(auto nex:v[u])
    {
        if(vis[nex])continue;
        dfs(nex,pos+1,st);
    }
}
int main()
{
    // io;
	int n;
    scanf("%d",&n);
    if(n==1)
    {
        cout<<1;
        return 0;
    }
	int a,b;
	for(int i=1;i<=n;i++)fa[i]=i;
	for(int i=1;i<n;i++)
	{
        scanf("%d%d",&a,&b);
        v[a].push_back(b);
        v[b].push_back(a);
		join(a,b);
	 } 
	 int num=0;
	 for(int i=1;i<=n;i++)
	 {
	 	if(i==fa[i])num++;
	 }
	 if(num!=1)cout<<"Error: "<<num<<" components\n";
	 else
	 {
        root=1;
        dfs(root,1,s1);
        maxp=0;
        for(auto u:s1)
        {
            if(maxp==0)
            {
                memset(vis,0,sizeof vis);
                dfs(u,1,s2);
            }
            s2.insert(u);
        }
        for(auto u:s2)
        {
            printf("%d\n",u);
        }
	 }
 } 
```