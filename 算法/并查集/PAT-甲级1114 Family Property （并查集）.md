### Question:
This time, you are supposed to help us collect the data for family-owned property. Given each person's family members, and the estate（房产）info under his/her own name, we need to know the size of each family, and the average area and number of sets of their real estate.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤1000). Then N lines follow, each gives the infomation of a person who owns estate in the format:

`ID` `Father` `Mother` k Child1​⋯Childk​ Mestate​ `Area`

where `ID` is a unique 4-digit identification number for each person; `Father` and `Mother` are the `ID`'s of this person's parents (if a parent has passed away, `-1` will be given instead); k (0≤k≤5) is the number of children of this person; Childi​'s are the `ID`'s of his/her children; Mestate​ is the total number of sets of the real estate under his/her name; and `Area` is the total area of his/her estate.

### Output Specification:

For each case, first print in a line the number of families (all the people that are related directly or indirectly are considered in the same family). Then output the family info in the format:

`ID` `M` AVGsets​ AVGarea​

where `ID` is the smallest ID in the family; `M` is the total number of family members; AVGsets​ is the average number of sets of their real estate; and AVGarea​ is the average area. The average numbers must be accurate up to 3 decimal places. The families must be given in descending order of their average areas, and in ascending order of the ID's if there is a tie.

### Sample Input:

```in
10
6666 5551 5552 1 7777 1 100
1234 5678 9012 1 0002 2 300
8888 -1 -1 0 1 1000
2468 0001 0004 1 2222 1 500
7777 6666 -1 0 2 300
3721 -1 -1 1 2333 2 150
9012 -1 -1 3 1236 1235 1234 1 100
1235 5678 9012 0 1 50
2222 1236 2468 2 6661 6662 1 300
2333 -1 3721 3 6661 6662 6663 1 100
```

### Sample Output:

```out
3
8888 1 1.000 1000.000
0001 15 0.600 100.000
5551 4 0.750 100.000
```

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
const int N = 1e4+5;
const int M = 1e5;
const int mod = 1e9 + 7;
struct Datt
{
	int id,peo;
	double num,area;
	bool isrt;
	bool operator <(const Datt &g)const
	{
		if(area!=g.area)return area>g.area;
		else return id<g.id;
	}
}ans[N];
struct Node
{
	int id,num,area;
}f[N];
int fa[N];
int find(int x){return x==fa[x]?x:fa[x]=find(fa[x]);}
void join(int a,int b)
{
	a=find(a);
	b=find(b);
	//节点号小的作父节点
	if(a<b)fa[b]=a;
	else if(a>b)fa[a]=b;
}
bool vis[N];
int main()
{
	// io;
	int n,m,k,x;
	cin>>n;
	for(int i=0;i<10000;i++)fa[i]=i;
	int cur,fa,mo,son,num,area;
	//先合并父子节点并存储下来。
	for(int i=1;i<=n;i++)
	{
		cin>>cur>>fa>>mo;
		vis[cur]=1;
		f[i].id=cur;
		if(fa!=-1)
		{
			vis[fa]=1;
			join(cur,fa);
		}
		if(mo!=-1)
		{
			vis[mo]=1;
			join(cur,mo);
		}
		cin>>k;
		while(k--)
		{
			cin>>son;
			vis[son]=1;
			join(cur,son);
		}
		cin>>f[i].num>>f[i].area;
	}
	//再一齐处理，将id，人数，面积一同更新到父节点上去
	for(int i=1,rt;i<=n;i++)
	{
		rt=find(f[i].id);
		ans[rt].isrt=1; //根节点
		ans[rt].num+=f[i].num;
		ans[rt].area+=f[i].area;
		ans[rt].id=rt;
	}
	int cnt=0;
	for(int i=0;i<10000;i++)
	{
		if(vis[i])
		{
			ans[find(i)].peo++;
		}
		if(ans[i].isrt)cnt++;
	}
	vector<Datt> res;
	for(int i=0;i<10000;i++)
	{
		if(ans[i].isrt)
		{
			ans[i].num/=ans[i].peo;
			ans[i].area/=ans[i].peo;
			res.push_back(ans[i]);
		}
	}
	sort(res.begin(),res.end());
	cout<<cnt<<"\n";
	for(auto u:res)
	{
		printf("%04d %d %.3f %.3f\n",u.id,u.peo,u.num,u.area);
	}
	// system("pause");
	return 0;
}
/*

*/

```