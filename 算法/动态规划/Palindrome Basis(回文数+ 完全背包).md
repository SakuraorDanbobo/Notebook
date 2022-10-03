 #### Question:
 You are given a positive integer n. Let's call some positive integer a without leading zeroes palindromic if it remains the same after reversing the order of its digits. Find the number of distinct ways to express n as a sum of positive palindromic integers. Two ways are considered different if the frequency of at least one palindromic integer is different in them. For example, 5=4+1 and 5=3+1+1 are considered different but 5=3+1+1 and 5=1+3+1 are considered the same.

Formally, you need to find the number of distinct multisets of positive palindromic integers the sum of which is equal to n.

Since the answer can be quite large, print it modulo 10e9+7.

#### Input:
The first line of input contains a single integer t (1≤t≤104) denoting the number of testcases.

Each testcase contains a single line of input containing a single integer n (1≤n≤4⋅10e4) — the required sum of palindromic integers.

#### Output:
For each testcase, print a single integer denoting the required answer modulo 10e9+7.

#### Example:
**input**
2
5
12
**output**
7
74
#### Note:
For the first testcase, there are 7 ways to partition 5 as a sum of positive palindromic integers:
* 5=1+1+1+1+1
* 5=1+1+1+2
* 5=1+2+2
* 5=1+1+3
* 5=2+3
* 5=1+4
* 5=5
For the second testcase, there are total 77 ways to partition 12 as a sum of positive integers but among them, the partitions 12=2+10, 12=1+1+10 and 12=12 are not valid partitions of 12 as a sum of positive palindromic integers because 10 and 12 are not palindromic. So, there are 74 ways to partition 12 as a sum of positive palindromic integers.

#### 题意：
给你一个数n(n<=4e4),问从1~n中有多少种取m个是回文数的相加和等于n，同时要保证m个数的单调性。换而言之，在1~n中任取k个是回文数的数（可以多次取），最后相加能等于n的总取法，因为结果会很大，因此需要mod（10e9+7）。
#### 思路：
那么这道题理解了就很容易了，分2步骤走：
1. 首先打表1~n的回文数，因为这里n不大，最多只有4e4，因此直接1到n枚举。若n较大时，我们可以对半边进行枚举拼接。
2. 直接上裸板子的完全背包打表即可。

#### 参考代码：
```c++
#include <bits/stdc++.h>

#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)

#define LL long long

#define x first

#define y second

#define PII pair<int,int>

#define PDD pair<double,double>

using namespace std;

const int INF=0x3f3f3f3f;

const int N=4e4+5;

const int M=1e7;

const int mod=1e9+7;

int t,n,m,pos;

int p[N];

LL dp[N];

bool ok(int x)

{

    int ans=x,sum=0;

    while(x)

    {

        sum=sum*10+x%10;

        x/=10;

    }

    return ans==sum;

}

void init()

{

    int maxn=4e4;

    pos=0;

    for(int i=1;i<=maxn;i++)

    {

        if(ok(i))

        {

            p[++pos]=i;

        }

    }

    dp[0]=1;

    for(int i=1;i<=pos;i++)

    {

        for(int j=p[i];j<=maxn;j++)

         {

             dp[j]=(dp[j]+dp[j-p[i]])%mod;

         }

     }

}

int main()

{

    // io;

    init();

  

    cin>>t;

    while(t--)

    {

        cin>>n;

        cout<<dp[n]<<"\n";

    }

    system("pause");

    return 0;

}

/*

*/

```