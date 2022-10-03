There is a public bike service in Hangzhou City which provides great convenience to the tourists from all over the world. One may rent a bike at any station and return it to any other stations in the city.

The Public Bike Management Center (PBMC) keeps monitoring the real-time capacity of all the stations. A station is said to be in **perfect** condition if it is exactly half-full. If a station is full or empty, PBMC will collect or send bikes to adjust the condition of that station to perfect. And more, all the stations on the way will be adjusted as well.

When a problem station is reported, PBMC will always choose the shortest path to reach that station. If there are more than one shortest path, the one that requires the least number of bikes sent from PBMC will be chosen.

![[Pasted image 20220521135022.png]]

The above figure illustrates an example. The stations are represented by vertices and the roads correspond to the edges. The number on an edge is the time taken to reach one end station from another. The number written inside a vertex S is the current number of bikes stored at S. Given that the maximum capacity of each station is 10. To solve the problem at S3​, we have 2 different shortest paths:

1.  PBMC -> S1​ -> S3​. In this case, 4 bikes must be sent from PBMC, because we can collect 1 bike from S1​ and then take 5 bikes to S3​, so that both stations will be in perfect conditions.
    
2.  PBMC -> S2​ -> S3​. This path requires the same time as path 1, but only 3 bikes sent from PBMC and hence is the one that will be chosen.
    

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 numbers: Cmax​ (≤100), always an even number, is the maximum capacity of each station; N (≤500), the total number of stations; Sp​, the index of the problem station (the stations are numbered from 1 to N, and PBMC is represented by the vertex 0); and M, the number of roads. The second line contains N non-negative numbers Ci​ (i=1,⋯,N) where each Ci​ is the current number of bikes at Si​ respectively. Then M lines follow, each contains 3 numbers: Si​, Sj​, and Tij​ which describe the time Tij​ taken to move betwen stations Si​ and Sj​. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print your results in one line. First output the number of bikes that PBMC must send. Then after one space, output the path in the format: 0−>S1​−>⋯−>Sp​. Finally after another space, output the number of bikes that we must take back to PBMC after the condition of Sp​ is adjusted to perfect.

Note that if such a path is not unique, output the one that requires minimum number of bikes that we must take back to PBMC. The judge's data guarantee that such a path is unique.

### Sample Input:

```in
10 3 3 5
6 7 0
0 1 1
0 2 1
0 3 3
1 3 1
2 3 1
```

### Sample Output:

```out
3 0->2->3 0
```

### 参考代码：
```c++
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

const int P=13331;

  

int c,n,m,ed;

int num[555];

vector<PII> v[555];

int dis[555];

vector<int>pre[555];

//找出所有的最短路径条数并记录。

void dijkstra()

{

    memset(dis,INF,sizeof dis);

    priority_queue<PII,vector<PII>,greater<PII> >heap;

    dis[0]=0;

    heap.push({0,0});

    while(heap.size())

    {

        PII tmp=heap.top();

        heap.pop();

        int u=tmp.second;

        if(dis[u]<tmp.first)continue;

        for(auto go:v[u])

        {

            int j= go.first;

            int co=go.second;

            if(dis[j]>dis[u]+co)

            {

                dis[j]=dis[u]+co;

                pre[j].clear();

                pre[j].push_back(u);

                heap.push({dis[j],j});

            }

            else if(dis[j]==dis[u]+co)

            {

                pre[j].push_back(u);

            }

        }

    }

}

vector<int>path,tmppath;

int minsend=INF,minback=INF;

/*

    搜索每一条路径，并找到最小发送数量，若最小发送数量一样，

    则找最小收回数量

    这里我们倒着找

*/

void dfs(int u)

{

    tmppath.push_back(u);

    if(u==0)

    {

        int send=0,back=0;

        int len=tmppath.size();

        int k=c/2;

        for(auto i:tmppath)

        {

            if(i==0)continue;

            if(num[i]<k)

            {

                send+=(k-num[i]);

            }

            else if(num[i]>k)

            {

                back+=(num[i]-k);

                if(send!=0)

                {

                    int d=min(send,back);

                    send-=d;

                    back-=d;

                }

            }

        }        

        if(minsend>send)

        {

            minsend=send;

            minback=back;

            path=tmppath;

        }

        else if(minsend==send && minback>back)

        {

            minback=back;

            path=tmppath;

        }

  
  

        tmppath.pop_back();

        return ;

    }

    for(auto nex:pre[u])

    {

        dfs(nex);

    }

    tmppath.pop_back();

}

int main()

{

    // io;

    cin>>c>>n>>ed>>m;

    for( int i=1;i<=n;i++)

    {

        cin>>num[i];

    }

  

    while(m--)

    {

        int a,b,d;

        cin>>a>>b>>d;

        v[a].push_back({b,d});

        v[b].push_back({a,d});

    }

  

    dijkstra();

    dfs(ed);

  

    cout<<minsend<<" 0";

    for(int i=path.size()-2;i>=0;i--)

    {

        cout<<"->"<<path[i];

    }

    cout<<" "<<minback;

    system("pause");

    return 0;

}
```