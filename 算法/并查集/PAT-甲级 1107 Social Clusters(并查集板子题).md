### Question:
When register on a social network, you are always asked to specify your hobbies in order to find some potential friends with the same hobbies. A **social cluster** is a set of people who have some of their hobbies in common. You are supposed to find all the clusters.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains a positive integer N (≤1000), the total number of people in a social network. Hence the people are numbered from 1 to N. Then N lines follow, each gives the hobby list of a person in the format:

Ki : hi[1] hi[2] ... hi[Ki]

where Ki (>0) is the number of hobbies, and hi[j] is the index of the j-th hobby, which is an integer in [1, 1000].

### Output Specification:

For each case, print in one line the total number of clusters in the network. Then in the second line, print the numbers of people in the clusters in non-increasing order. The numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

### Sample Input:

```in
8
3: 2 7 10
1: 4
2: 5 3
1: 4
1: 3
1: 4
4: 6 8 1 5
1: 4
```

### Sample Output:

```out
3
4 3 1
```

### 题意:
```in
给你n个人的兴趣爱好，用数字表示[1,1000]。若2个的爱好中有一个一样，则他们是一个群体。最后求总的群体数，以及降序输出每个群体的人数。
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
const int N = 1e5 + 5;
const int M = 2e5+5;
const int mod = 1e9 + 7;

 
int fa[1111],vis[1111];
int find(int x){return fa[x]==x?x:fa[x]=find(fa[x]);}
void join(int a,int b){fa[find(b)]=find(a);}
int main()
{
	int n;
	cin>>n;
	for(int i=1,k,x;i<=n;i++)
	{
		fa[i]=i;

		scanf("%d: ",&k);
		while(k--)
		{
			cin>>x;
			if(vis[x]==0)vis[x]=i;
			else
			{
				join(i,vis[x]);
			}
		}
	}
	int cnt=0;
	map<int,int>m;
	for(int i=1;i<=n;i++)
	{
		find(i);
		m[fa[i]]++;
		if(i==fa[i])cnt++;
		// cout<<i<<" "<<fa[i]<<"\n";
	}
	cout<<cnt<<"\n";
	vector<int>v;
	for(auto u:m)
	{
		v.push_back(u.second);
	}
	sort(v.rbegin(),v.rend());
	for(int i=0;i<cnt;i++)
	{
		if(i!=0)cout<<" ";
		cout<<v[i];
	}
	// system("pause");
	return 0;
	
}
	
/*
5
2 2 3
2 1 5
1 4
1 3
0
111
*/

```