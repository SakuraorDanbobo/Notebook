**描述**

Saruman the White must lead his army along a straight path from Isengard to Helm’s Deep. To keep track of his forces, Saruman distributes seeing stones, known as palantirs, among the troops. Each palantir has a maximum effective range of *R* units, and must be carried by some troop in the army (i.e., palantirs are not allowed to “free float” in mid-air). Help Saruman take control of Middle Earth by determining the minimum number of palantirs needed for Saruman to ensure that each of his minions is within *R* units of some palantir.

**输入**

The input test file will contain multiple cases. Each test case begins with a single line containing an integer *R*, the maximum effective range of all palantirs (where 0 ≤ *R* ≤ 1000), and an integer *n*, the number of troops in Saruman’s army (where 1 ≤ *n* ≤ 1000). The next line contains n integers, indicating the positions *x*1, …, *xn* of each troop (where 0 ≤ *xi* ≤ 1000). The end-of-file is marked by a test case with *R* = *n* = −1.

**输出**

For each test case, print a single integer indicating the minimum number of palantirs needed.

**样例输入**

0 3
10 20 20
10 7
70 30 1 7 15 20 50
-1 -1

**样例输出**

2
4

**提示**

In the first test case, Saruman may place a palantir at positions 10 and 20. Here, note that a single palantir with range 0 can cover both of the troops at position 20.

In the second test case, Saruman can place palantirs at position 7 (covering troops at 1, 7, and 15), position 20 (covering positions 20 and 30), position 50, and position 70. Here, note that palantirs must be distributed among troops and are not allowed to “free float.” Thus, Saruman cannot place a palantir at position 60 to cover the troops at positions 50 and 70.

 

题意：给你n个点的位置（不一定有序），再给定一个r（左右范围），在n个点中选择几个标记点，使所有点都在标记点的覆盖下，问最少需要标记几个点。



思路：先排序位置大小，接着从左往右搜，然后从第一个点开始，先找到右边r范围内的最大点作为标记点，接着标记点右侧r范围内的点都可以跳过，不断往复得到最后答案

```c++
#include <bits/stdc++.h>
#define x first
#define y second
#define LL long long
#define PII pair<int,int>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=1e6;
int f[1002];
int main()
{
    int r,n; 
    while(cin>>r>>n && (r+n!=-2))
    {
    	for(int i=1;i<=n;i++)cin>>f[i];
    	
    	//位置是无序的，因此需要从小到大排序 
    	sort(f+1,f+1+n);
    	
    	int pos=1,ans=0;
    	while(pos<=n)
    	{
    		//当前位置点
			int num=f[pos]; 
    		pos++;
    		
    		//找R范围内最远的点,将该点标记后，在去搜标记点右边范围内的点 
    		while(pos<=n && f[pos]<=num+r)pos++;
    		
			ans++; //标记点+1
			
			num=f[pos-1]; //标记点的位置 
			while(pos<=n && f[pos]<=num+r)pos++;
		}
		cout<<ans<<"\n";
	}
    return 0;
}


```

