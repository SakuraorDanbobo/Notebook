
  
#### 问题
给定一张 N 个点（编号 1,2…N），M 条边的有向图，求从起点 S 到终点 T 的第 K 短路的长度，路径允许重复经过点或边。

**注意：** 每条最短路中至少要包含一条边。

#### 输入格式

第一行包含两个整数 N 和 M。

接下来 M 行，每行包含三个整数 A,B 和 L，表示点 A 与点 B 之间存在有向边，且边长为 L。

最后一行包含三个整数 S,T 和 K，分别表示起点 S，终点 T 和第 K 短路。

#### 输出格式

输出占一行，包含一个整数，表示第 K 短路的长度，如果第 K 短路不存在，则输出 −1。

#### 数据范围

1≤S,T≤N≤1000,  
0≤M≤10e4,  
1≤K≤1000,  
1≤L≤100

#### 输入样例：

```
2 2
1 2 5
2 1 4
1 2 2
```

#### 输出样例：

```
14
```


#### 参考代码

```c++
#include <bits/stdc++.h>

#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)

#define LL long long

#define x first

#define y second

#define PII pair<int,int>

#define PIII pair<int,PII>

#define PDD pair<double,double>

using namespace std;

const int INF=0x3f3f3f3f;

const int N=1005;

const int M=1e7;

const int mod=1e9+7;

  

int n,m,st,ed,k;

vector<PII>h[N],rh[N]; //正向边和反向边

int dis[N]; //终点到各点的距离，用来作h(n)

/*

思路：

    1.跑一遍dijkstra反向边来求得终点T到各个点的距离，用来当做预估值h(n)

    2.A*求解, f(n)=g(n)+h(n); 适用于边权为正的有向图最短距离问题。

        g(n)实际代价，h(n)预估代价

    思考：当h(n)=0时，f(n)所求的即是地杰斯特拉最短路。

    其次要保证h(n)<=g(n)[必要条件]，若是不满足条件，则第一个出队列的不一定是最优值。

*/

  

void dijkstra()

{

    priority_queue<PII,vector<PII>,greater<PII> >q;

    memset(dis,INF,sizeof dis);

    dis[ed]=0;

    q.push({0,ed});

    while(q.size())

    {

        auto tmp=q.top();

        q.pop();

        int c=tmp.y;

        if(tmp.x>dis[c])continue;

  

        for(auto val:rh[c])

        {

            if(dis[val.x]>dis[c]+val.y)

            {

                dis[val.x]=dis[c]+val.y;

                q.push({dis[val.x],val.x});

            }

        }

    }

}

  

int astar()

{

    //特判起点到终点能否到达

    if(dis[st]==INF)return -1;

    //PIII<预估距离，<实际距离，当前点> >

    priority_queue<PIII,vector<PIII>,greater<PIII> >q;

    q.push({dis[st],{0,st}});

    int cnt=0; //判断第几次到终点，即第K短路

    while(q.size())

    {

        auto tmp=q.top();

        q.pop();

        //实际距离

        int tdis=tmp.y.x;

        //当前点

        int u=tmp.y.y;

        if(u==ed)cnt++;

        if(cnt==k)return tdis;

        for(auto val : h[u])

        {

            //tdis+val.y 实际距离

            //tdis+val.y+dis[val.x] 预估距离

            q.push({tdis+val.y+dis[val.x],{tdis+val.y,val.x}});

        }

    }

    //若不存在第k短路

    return -1;

}

int main()

{

    cin>>n>>m;

    for(int i=0,a,b,l;i<m;i++)

    {

        cin>>a>>b>>l;

        h[a].push_back({b,l});

        rh[b].push_back({a,l});

    }

    cin>>st>>ed>>k;

    if(st==ed)k++;

    dijkstra();

    cout<<astar()<<"\n";

  

    system("pause");

    return 0;

}

```