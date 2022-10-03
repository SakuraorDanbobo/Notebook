小明负责维护公司一个奇怪的项目。这个项目的代码一直在不断分支(branch)但是从未发生过合并(merge)。  
　　现在这个项目的代码一共有N个版本，编号1~N，其中1号版本是最初的版本。  
　　除了1号版本之外，其他版本的代码都恰好有一个直接的父版本；即这N个版本形成了一棵以1为根的树形结构。  
  
　　如下图就是一个可能的版本树：  
  
 1  
/  \\  
2  3  
|   / \  
5 4 6  
  
  现在小明需要经常检查版本x是不是版本y的祖先版本。你能帮助小明吗
   
输入格式
　　第一行包含两个整数N和Q，代表版本总数和查询总数。  
　　以下N-1行，每行包含2个整数u和v，代表版本u是版本v的直接父版本。  
　　再之后Q行，每行包含2个整数x和y，代表询问版本x是不是版本y的祖先版本。  
  
　　对于30%的数据，1 <= N <= 1000 1 <= Q <= 1000  
　　对于100%的数据，1 <= N <= 100000 1 <= Q <= 100000

输出格式

　　对于每个询问，输出YES或NO代表x是否是y的祖先。

样例输入

6 5  
1 2  
1 3  
2 5  
3 6  
3 4  
1 1  
1 4  
2 6  
5 2  
6 4

样例输出

YES  
YES  
NO  
NO  
NO

### 参考代码:
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
const int N = 2e5 + 5;
const int M = 2e5 + 5;
const int mod = 1e9 + 7;

vector<int> e[N];
int lg[N], fa[N][22], dis[N];
int n, m;
inline void dfs(int x, int fath)
{
	fa[x][0] = fath;
	dis[x] = dis[fath] + 1;
	for (int i = 1; i <= lg[dis[x]]; i++)
		fa[x][i] = fa[fa[x][i - 1]][i - 1];
	for (int i = 0; i < e[x].size(); i++)
	{
		if (e[x][i] != fath)
			dfs(e[x][i], x);
	}
}
inline int LCA(int x, int y)
{
	if (dis[x] < dis[y])
		swap(x, y);
	while (dis[x] > dis[y])
		x = fa[x][lg[dis[x] - dis[y]] - 1];
	if (x == y)
		return x;
	for (int k = lg[dis[x]] - 1; k >= 0; k--)
		if (fa[x][k] != fa[y][k])
			x = fa[x][k], y = fa[y][k];
	return fa[x][0];
}
int main()
{
	scanf("%d%d", &n, &m);
	for (int i = 1; i < n; i++)
	{
		int a, b;
		scanf("%d%d", &a, &b);
		e[a].push_back(b);
		e[b].push_back(a);
	}
	for (int i = 1; i <= n; i++)
		lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
	dfs(1, 0);
	for (int i = 1; i <= m; i++)
	{
		int a, b;
		scanf("%d%d", &a, &b);
		int c = LCA(a, b);
		if (c == a)
			printf("YES\n");
		else
			printf("NO\n");
	}
	return 0;
}

```