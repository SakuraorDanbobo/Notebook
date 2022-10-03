
### 描述
In the year 2008, the 29th Olympic Games will be held in Beijing. This will signify the prosperity of China and Beijing Olympics is to be a festival for people all over the world as well.
Liu Xiang is one of the famous Olympic athletes in China. In 2002 Liu broke Renaldo Nehemiah's 24-year-old world junior record for the 110m hurdles. At the 2004 Athens Olympics Games, he won the gold medal in the end. Although he was not considered as a favorite for the gold, in the final, Liu's technique was nearly perfect as he barely touched the sixth hurdle and cleared all of the others cleanly. He powered to a victory of almost three meters. In doing so, he tied the 11-year-old world record of 12.91 seconds. Liu was the first Chinese man to win an Olympic gold medal in track and field. Only 21 years old at the time of his victory, Liu vowed to defend his title when the Olympics come to Beijing in 2008.
In the 110m hurdle competition, the track was divided into _N_ parts by the hurdle. In each part, the player has to run in the same speed; otherwise he may hit the hurdle. In fact, there are 3 modes to choose in each part for an athlete -- Fast Mode, Normal Mode and Slow Mode. Fast Mode costs the player _T1_ time to pass the part. However, he cannot always use this mode in all parts, because he needs to consume _F1_ force at the same time. If he doesn't have enough force, he cannot run in the part at the Fast Mode. Normal Mode costs the player _T2_ time for the part. And at this mode, the player's force will remain unchanged. Slow Mode costs the player _T3_ time to pass the part. Meanwhile, the player will earn _F2_ force as compensation. The maximal force of a player is _M_. If he already has _M_ force, he cannot earn any more force. At the beginning of the competition, the player has the maximal force.
The input of this problem is detail data for Liu Xiang. Your task is to help him to choose proper mode in each part to finish the competition in the shortest time.
### 输入
Standard input will contain multiple test cases. The first line of the input is a single integer _T_ (1 <= _T_ <= 50) which is the number of test cases. And it will be followed by _T_ consecutive test cases.
Each test case begins with two positive integers _N_ and _M_. And following _N_ lines denote the data for the _N_ parts. Each line has five positive integers _T1 T2 T3 F1 F2_. All the integers in this problem are less than or equal to 110.
### 输出
Results should be directed to standard output. The output of each test case should be a single integer in one line, which is the shortest time that Liu Xiang can finish the competition.

### 样例输入
```in
2
1 10
1 2 3 10 10
4 10
1 2 3 10 10
1 10 10 10 10
1 1 2 10 10
1 10 10 10 10
```

### 样例输出
```in
1
6
```
### 提示
For the second sample test case, Liu Xiang should run with the sequence of Normal Mode, Fast Mode, Slow Mode and Fast Mode.

### 思路:
```in
状态定义：dp[i][j]表示前i个赛道拥有j能量的花费最小时间
状态转移有以下3种：
1.快，需要花费f0的体力，需要的时间是t0。
p为消耗体力后剩余的体力，即p=j-f[0]。
仅当p>=0时，dp[i][p]=min(dp[i][p],dp[i-1][j]+t[0]);
2.中，不需要花费体力，需要时间是t1
dp[i][j]=min(dp[i][j],dp[i-1][j]+t[1]);
3.慢，加f2的体力，需要的时间是t2
all为增加体力后的体力，即all=j+f[2],同时需要保证all不溢出总体力m，即min(all,m)
dp[i][all]=min(dp[i][all],dp[i-1][j]+t[2]);
```
### 参考代码：
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PLL pair<LL, LL>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 4e6 + 5;
const int M = 1e5;
const int mod = 1e9 + 7;

int dp[222][222]; //前i个赛道拥有j能量的花费最小时间
int t[3],f[3];
void solve()
{
	int n,m,t1,t2,t3,f1,f2;
	cin>>n>>m;
	memset(dp,INF,sizeof dp);
	for(int i=0;i<=m;i++)dp[0][i]=0;
	for(int i=1;i<=n;i++)
	{
		cin>>t[0]>>t[1]>>t[2]>>f[0]>>f[2];
		for(int j=0;j<=m;j++)
		{
			//加速
			int p=j-f[0];
			if(p>=0)dp[i][p]=min(dp[i][p],dp[i-1][j]+t[0]);
			
			//匀速
			dp[i][j]=min(dp[i][j],dp[i-1][j]+t[1]);
			//慢速
			int all=min(m,j+f[2]);
			dp[i][all]=min(dp[i][all],dp[i-1][j]+t[2]);
		} 
		 
	}
	int res=INF;
	for(int i=0;i<m;i++)
	{
		res=min(res,dp[n][i]);
	}
	cout<<res<<"\n";
}
int main()
{
	// io;
	int t;
	int a,b;
	while(cin>>t)
	{
		 while(t--)
		 {
			solve();
		 }
	}
	

	
	// system("pause");
	return 0;
}
/*
3
1 10
5 2 3 10 10
4 10
1 2 3 10 10
1 10 10 10 10
1 1 2 10 10
1 10 10 10 10
3 10
1 1 1 3 10
1 1 1 10 10
1 1 2 3 10
*/

```