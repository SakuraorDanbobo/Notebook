#### 描述
都说天上不会掉馅饼，但有一天gameboy正走在回家的小径上，忽然天上掉下大把大把的馅饼。说来gameboy的人品实在是太好了，这馅饼别处都不掉，就掉落在他身旁的10米范围内。馅饼如果掉在了地上当然就不能吃了，所以gameboy马上卸下身上的背包去接。但由于小径两侧都不能站人，所以他只能在小径上接。由于gameboy平时老呆在房间里玩游戏，虽然在游戏中是个身手敏捷的高手，但在现实中运动神经特别迟钝，每秒种只有在移动不超过一米的范围内接住坠落的馅饼。现在给这条小径如图标上坐标：
![[Pasted image 20220508160320.png]]
为了使问题简化，假设在接下来的一段时间里，馅饼都掉落在0-10这11个位置。开始时gameboy站在5这个位置，因此在第一秒，他只能接到4,5,6这三个位置中其中一个位置上的馅饼。问gameboy最多可能接到多少个馅饼？（假设他的背包可以容纳无穷多个馅饼）
#### 输入
输入数据有多组。每组数据的第一行为以正整数n(0<n<100000)，表示有n个馅饼掉在这条小径上。在结下来的n行中，每行有两个整数x,T(0<T<100000),表示在第T秒有一个馅饼掉在x点上。同一秒钟在同一点上可能掉下多个馅饼。n=0时输入结束。

#### 输出
每一组输入数据对应一行输出。输出一个整数m，表示gameboy最多可能接到m个馅饼。
#### 样例输入
6
5 1
4 1
6 1
7 2
7 2
8 3
0
#### 样例输出
4

#### 思路
```in
类似数塔的操作，比如j+1秒的操作就是类似数塔下层的操作。
这里我们从下往上操作，也就是时间点倒着来取，这样一定能保证每次是从取了最优情况的下一秒递推回上一秒的。
定义状态方程:dp[i][j],第i个点，第j秒的最大饼数。
因为每次我们只能往相邻两个方向和待在原地
因此我们的状态转移方程: dp[i][j]=max(dp[i][j+1],dp[i+1][j+1],dp[i-1][j+1])
值得注意的是，我们要考虑到边界的情况（即0点和10点时候的情况）
那么最终,dp[5][0]即是我们最终的结果。
```
#### 参考代码
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define x first
#define y second
#define LL long long
#define PII pair<int, int>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1e5 + 5;
const int M = 1e7;
const int mod = 998244353;
const int add = 1e6;

PII f[N];
// dp[i][j]:第i个点第j秒的个数
int dp[12][100005];
int main()
{
    int n,a,b;
    while(cin>>n && n)
    {
        memset(dp,0,sizeof dp);
        int t=0;
        for(int i=1;i<=n;i++)
        {
            cin>>a>>b;
            dp[a][b]++;
            t=max(t,b);
        }
        t--;
        //类似树塔操作
        for(;t>=0;t--)
        {
            for(int i=0;i<=10;i++)
            {
                if(i==0)
                {
                    dp[i][t]+=max(dp[i][t+1],dp[i+1][t+1]);
                }
                else if(i==10)
                {
                    dp[i][t]+=max(dp[i][t+1],dp[i-1][t+1]);
                }
                else
                {
                    dp[i][t]+=max(dp[i][t+1],max(dp[i-1][t+1],dp[i+1][t+1]));
                }
            }
        }
        cout<<dp[5][0]<<"\n";
    }
    // system("pause");

    return 0;
}
```