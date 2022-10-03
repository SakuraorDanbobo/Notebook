### Question:
Given a singly linked list, you are supposed to rearrange its elements so that all the negative values appear before all of the non-negatives, and all the values in [0, K] appear before all those greater than K. The order of the elements inside each class must not be changed. For example, given the list being 18→7→-4→0→5→-6→10→11→-2 and K being 10, you must output -4→-6→-2→7→0→5→10→18→11.

### Input Specification:

Each input file contains one test case. For each case, the first line contains the address of the first node, a positive N (≤105) which is the total number of nodes, and a positive K (≤103). The address of a node is a 5-digit nonnegative integer, and NULL is represented by −1.

Then N lines follow, each describes a node in the format:

```
Address Data Next
```

where `Address` is the position of the node, `Data` is an integer in [−105,105], and `Next` is the position of the next node. It is guaranteed that the list is not empty.

### Output Specification:

For each case, output in order (from beginning to the end of the list) the resulting linked list. Each node occupies a line, and is printed in the same format as in the input.

### Sample Input:

```in
00100 9 10
23333 10 27777
00000 0 99999
00100 18 12309
68237 -6 23333
33218 -4 00000
48652 -2 -1
99999 5 68237
27777 11 48652
12309 7 33218
```

### Sample Output:

```out
33218 -4 68237
68237 -6 48652
48652 -2 12309
12309 7 00000
00000 0 99999
99999 5 23333
23333 10 00100
00100 18 27777
27777 11 -1
```

### 题意：
```in
给你一个链表和一个数字K，遍历链表后将<0的结点先输出，再将[0,k]区间的结点输出，最后输出>k的结点.
```
### 思路：
```in 
只要遍历一遍链表，再分别将3种情况(<0、[0,k]、>k)的地址对应存入v[0]、v[1]、v[2]中，最后将vector中的值依次输出即可。
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
 
int da[N];
int main()
{
	// io;
	int n,m,root,k,a,b,c;
	map<int,int>nx;
	cin>>root>>n>>k;
	for(int i=1;i<=n;i++)
	{
		cin>>a>>b>>c;
		da[a]=b;
		nx[a]=c;
	}
	//3部分，负数，[0,k]的数，大于k的数
	vector<int> p[3];
	while(root!=-1)
	{
		int c=da[root];
		if(c<0)p[0].push_back(root);
		else if(c<=k)p[1].push_back(root);
		else p[2].push_back(root);
		root=nx[root]; //指向下一个节点
	}
	bool first=1;
	for(int i=0;i<3;i++)
	{
		for(auto u:p[i])
		{
			if(first)
			{
				printf("%05d %d",u,da[u]);
				first=0;
			}
			else
			{
				printf(" %05d\n%05d %d",u,u,da[u]);
			}
		}
	}
	printf(" -1");
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
