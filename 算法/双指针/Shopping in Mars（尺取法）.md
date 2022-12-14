### Question:
Shopping in Mars is quite a different experience. The Mars people pay by chained diamonds. Each diamond has a value (in Mars dollars M$). When making the payment, the chain can be cut at any position for only once and some of the diamonds are taken off the chain one by one. Once a diamond is off the chain, it cannot be taken back. For example, if we have a chain of 8 diamonds with values M$3, 2, 1, 5, 4, 6, 8, 7, and we must pay M$15. We may have 3 options:

1.  Cut the chain between 4 and 6, and take off the diamonds from the position 1 to 5 (with values 3+2+1+5+4=15).
2.  Cut before 5 or after 6, and take off the diamonds from the position 4 to 6 (with values 5+4+6=15).
3.  Cut before 8, and take off the diamonds from the position 7 to 8 (with values 8+7=15).

Now given the chain of diamond values and the amount that a customer has to pay, you are supposed to list all the paying options for the customer.

If it is impossible to pay the exact amount, you must suggest solutions with minimum lost.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 numbers: N (≤105), the total number of diamonds on the chain, and M (≤108), the amount that the customer has to pay. Then the next line contains N positive numbers D1​⋯DN​ (Di​≤103 for all i=1,⋯,N) which are the values of the diamonds. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print `i-j` in a line for each pair of `i` ≤ `j` such that D`i` + ... + D`j` = M. Note that if there are more than one solution, all the solutions must be printed in increasing order of `i`.

If there is no solution, output `i-j` for pairs of `i` ≤ `j` such that D`i` + ... + D`j` >M with (D`i` + ... + D`j` −M) minimized. Again all the solutions must be printed in increasing order of `i`.

It is guaranteed that the total value of diamonds is sufficient to pay the given amount.

### Sample Input 1:

```in
16 15
3 2 1 5 4 6 8 7 16 10 15 11 9 12 14 13
```

### Sample Output 1:

```out
1-5
4-6
7-8
11-11
```

### Sample Input 2:

```in
5 13
2 4 5 7 9
```

### Sample Output 2:

```out
2-4
4-5
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
const int N = 1e5 + 5;
const int M = 1e5;
const int mod = 1e9 + 7;


int val[N];
int main()
{
    // io;
	int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)cin>>val[i];
    bool flag=0;
    int res=INF;
    set<PII>s;
    int l=1,r=1,sum=0;
    while(l<=n)
    {
        while(r<=n && sum<m)
        {
            sum+=val[r];
            r++;
        }
        if(sum==m)
        {
            if(!flag)s.clear();
            s.insert({l,r-1});
            flag=1;
        }
        else if(sum>m && !flag)
        {
            int d=sum-m;
            if(res>d)
            {
                s.clear();
                s.insert({l,r-1});
                res=d;
            }
            else if(res==d)s.insert({l,r-1});
        }
        sum-=val[l];
        l++;
    } 
    for(auto u:s)cout<<u.first<<"-"<<u.second<<"\n";
    
 } 
```