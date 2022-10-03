### 问题描述：
芬兰木棋（Mölkky，又称芬兰木柱）是源自芬兰的一项运动。哲哲将这个运动改造成了赛博朋克单人版，现在场上一开始有 N 根立起的小木棋（上面分别标有一个非负整数），哲哲投掷一根大木棋去击倒这些小木棋以获得分数。分数规则如下：

-   如果仅击倒 1 根木棋，则得木棋上的分数。
-   如果击倒 2 根或以上的木棋，则只得击倒根数的分数。（例如击倒 5 根，则得 5 分。）

哲哲固定站在 (0,0) 点上，四周放着若干个小木棋 (Xi,Yi)，坐标均为整数。每次哲哲可以朝一个方向扔出大木棋，大木棋会打倒这个方向上离哲哲最近的 k 个小木棋。哲哲游戏水平很高超，所以这个 k 可以自由控制。

请问哲哲最多能拿多少分，在获得最多分数的情况下最少需要扔出多少次大木棋？

_规则与真实规则有较大出入，真实游玩时请以国际莫尔基组织的规则为准_

### 输入格式:

输入第一行是一个正整数 N (1 ≤ N ≤ 10^5)，表示场上一开始有 N 个木棋。

接下来 N 行，每行 3 个整数 Xi,Yi,Pi，分别表示木棋放置在 (Xi,Yi)，木棋上的分数是 Pi。坐标在 32 位整数范围内，分数为小于等于 1000 的正整数。

保证 (0,0) 点没有木棋，也没有木棋重叠放置。

### 输出格式:

输出一行两个数，表示最多分数以及获得最多分数最少需要投掷大木棋多少次。

### 输入样例:

```in
11
1 2 2
2 4 3
3 6 4
-1 2 2
-2 4 3
-3 6 4
-1 -2 1
-2 -4 1
-3 -6 1
-4 -8 2
2 -1 999
```

### 输出样例:

```out
1022 9
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
const int N = 5e4 + 5;
const int M = 1e3+5;
const int mod = 1e9 + 7;
const int add = 1e6;

int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
}
bool cmp(PLL a, PLL b)
{
    return a.first<b.first;
}
int main()
{
	io;
	int n;
	LL x,y,c,sum=0;
	cin>>n;
	map<PLL,vector<PLL> >v;
	while(n--) 
	{
	    cin>>x>>y>>c;
	    sum+=c;
	    
	    int d=gcd(abs(x),abs(y));
	    v[{x/d,y/d}].push_back({x+y,c});
	}
	LL cnt=0;
	vector<PLL>tmp;
	for(auto val:v)
	{
	    tmp=val.second;
	    sort(tmp.begin(),tmp.end(),cmp);
	    for(int i=0;i<tmp.size();i++)
	    {
	        if(tmp[i].second!=1)cnt++;
	        else
	        {
	            if(i==0 || tmp[i-1].second!=1)cnt++;
	        }
	    }
	}
	cout<<sum<<" "<<cnt<<"\n";
	return 0;
}
```