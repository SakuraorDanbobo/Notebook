#### 描述

总公司拥有高效设备M台，准备分给下属的N个分公司。各分公司若获得这些设备，可以为国家提供一定的盈利。问：如何分配这M台设备才能使国家得到的盈利最大？求出最大盈利值。其中M≤15，N≤10。分配原则：每个公司有权获得任意数目的设备，但总台数不超过设备数M。

#### 输入

第一行有两个数，第一个数是分公司数N，第二个数是设备台数M；

接下来是一个N*M的矩阵，表明了第 I个公司分配 J台机器的盈利。



#### 输出

第一行输出最大盈利值；

接下N行，每行有2个数，即分公司编号和该分公司获得设备台数。



#### 样例输入
```in
3 3
30 40 50
20 30 50
20 25 30
```

#### 样例输出
```out
70
1 1
2 1
3 1
```

```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
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
 
int cost[66][66];
int dp[66][66]; //dp[i][j]:前i个公司分配j台机器的最大收益。
//逆推打印
void dfs(int i,int j,int sum)
{
    if(!i)return;
    //当前取的设备数
    for(int k=0;k<=j;k++)
    {
        if(sum==dp[i-1][j-k]+cost[i][k])
        {
            dfs(i-1,j-k,dp[i-1][j-k]);
            cout<<i<<" "<<k<<"\n";
            break;
        }
    }
}
int main()
{
    io;
     int n,m;
     cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            cin>>cost[i][j];
        }
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            //当前取的设备数
            for(int k=0;k<=j;k++)
            {
                dp[i][j]=max(dp[i][j],dp[i-1][j-k]+cost[i][k]);
            }
            // cout<<dp[i][j]<<" ";
        }
        // cout<<"\n";
    }
    cout<<dp[n][m]<<"\n";
    dfs(n,m,dp[n][m]);
    // system("pause");
    return 0;
}
/*
 
*/
```