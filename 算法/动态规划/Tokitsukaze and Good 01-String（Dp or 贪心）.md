#### Question
This is the hard version of the problem. The only difference between the two versions is that the harder version asks additionally for a minimum number of subsegments.

Tokitsukaze has a binary string s of length n, consisting only of zeros and ones, n is even.

Now Tokitsukaze divides s into the minimum number of contiguous subsegments, and for each subsegment, all bits in each subsegment are the same. After that, s is considered good if the lengths of all subsegments are even.

For example, if s is "11001111", it will be divided into "11", "00" and "1111". Their lengths are 2, 2, 4 respectively, which are all even numbers, so "11001111" is good. Another example, if s is "1110011000", it will be divided into "111", "00", "11" and "000", and their lengths are 3, 2, 2, 3. Obviously, "1110011000" is not good.

Tokitsukaze wants to make s good by changing the values of some positions in s. Specifically, she can perform the operation any number of times: change the value of si to '0' or '1' (1≤i≤n). Can you tell her the minimum number of operations to make s good? Meanwhile, she also wants to know the minimum number of subsegments that s can be divided into among all solutions with the minimum number of operations.

#### Input
The first contains a single positive integer t (1≤t≤10000) — the number of test cases.

For each test case, the first line contains a single integer n (2≤n≤2⋅105) — the length of s, it is guaranteed that n is even.

The second line contains a binary string s of length n, consisting only of zeros and ones.

It is guaranteed that the sum of n over all test cases does not exceed 2⋅105.

#### Output
For each test case, print a single line with two integers — the minimum number of operations to make s good, and the minimum number of subsegments that s can be divided into among all solutions with the minimum number of operations.
##### input
```in
5
10
1110011000
8
11001111
2
00
2
11
6
100110
```
#### output
```out
3 2
0 3
0 1
0 1
3 1
```
#### Note
In the first test case, one of the ways to make s good is the following.

Change s3, s6 and s7 to '0', after that s becomes "1100000000", it can be divided into "11" and "00000000", which lengths are 2 and 8 respectively, the number of subsegments of it is 2. There are other ways to operate 3 times to make s good, such as "1111110000", "1100001100", "1111001100", the number of subsegments of them are 2, 4, 4 respectively. It's easy to find that the minimum number of subsegments among all solutions with the minimum number of operations is 2.

In the second, third and fourth test cases, s is good initially, so no operation is required.

#### 题意
```in
给你一段偶数长度为n的01串，要保证每段都是偶数个(即0连续的个数和1连续的个数为偶数个)
1.问最少修改几次
2.问最少可以划分几段
```
> 题解指路：https://codeforces.com/contest/1678/attachments/download/16089/Codeforces%20Round%20789%20Chinese%20Tutorial.pdf
> 题目指路：https://codeforces.com/contest/1678/problem/B2
> CF789,DIV2 ,B2题
#### 思路
```in
首先修改次数，题目保证偶数对，因此我们只需要对每2位进行判断是否相同，若不同则我们的操作数就需要+1即可。
其次，划分段的做法分两种，dp和贪心。
1.D：显然，操作方式是对于每一对相邻且不等的字符，把它们都变成0或者都变成1。换言之，将字符串分成许多相邻的 长度为2的二元组。 考虑从前向后的递推，可以发现对于前面已经维护好的前缀一定以"00"或"11"结尾，所以需要将每一个划分出来的 二元组转换成"00"或"11"，通过枚举当前二元组变化成"00"，"11"的代价，在DP的过程中维护改变次数最少的情况 下，尽可能的使得当前串与前缀子段的结尾一致。
状态定义：dp[n][2],长度为i的后缀状态为j的最小段数

状态转移：
dp[i][0]=min(dp[i−2][0]+{c0,0},dp[i−2][1]+{c0,1})
dp[i][1]=min(dp[i−2][0]+{c1,1},dp[i−2][1]+{c1,0})
其中ck表示为当前二元组转换为"kk"的代价。

2.贪：一个老贪比应该清楚如何正确的贪，我们发现对"01"或"10"进行操作并不会有太多贡献，而统计有多少连续的"11"二元组和"00"二元组有着更好的效果。

```

#### 参考代码：
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 2e5 + 5;
const int M = 1e6;
const int mod = 1e9 + 7;
const int add = 1e6;

int t, n;
string str;
/*
    需要值得注意是串的长度为偶数个，因此我们可拆分为n个二元组，来分别进行判断。
    方法主要分2种，动态规划和贪心。
*/
// 1.贪心
int greedy()
{
    //最小段数
    int cnt=0;

    char pre = ' '; //前一段的字符
    for (int i = 0; i < n; i += 2)
    {
        //因为对"01"或"10"进行操作并不会有太多贡献
        //统计有多少连续的"11"二元组和"00"二元组即可。
        if (str[i] == str[i + 1] && str[i] != pre)
        {
            pre = str[i];
            cnt++;
        }
    }
    //操作次数为0时的1段和存在操作次数分割的段数
    cnt = max(1, cnt);

    return cnt;
}

// 2.动态规划
int dp[N][2]; //dp[i][j],长度为i的后缀状态为j的最小段数。
/*
dp[i][0]=min(dp[i−2][0]+{c0,0},dp[i−2][1]+{c0,1})
dp[i][1]=min(dp[i−2][0]+{c1,1},dp[i−2][1]+{c1,0})
*/
int dppp()
{
    memset(dp,INF,sizeof dp);
    for(int i=1;i<n;i+=2)
    {
        //当前二元组相同时
        if(str[i]==str[i-1])
        {
            //相同共成1段
            if(i==1)dp[i][str[i]-'0']=1;
            else
            {
                if(str[i]=='1')dp[i][1]=min(dp[i-2][1],dp[i-2][0]+1);
                else dp[i][0]=min(dp[i-2][1]+1,dp[i-2][0]);
            }
        }
        //当前二元组不相同时
        else
        {
            //不同，以0和1结尾各成1段
            if(i==1)dp[i][0]=dp[i][1]=1;
            else
            {
                dp[i][1]=min(dp[i-2][1],dp[i-2][0]+1);
                dp[i][0]=min(dp[i-2][1]+1,dp[i-2][0]);
            }
        }
         
    }
    return min(dp[n-1][0],dp[n-1][1]);
}
int main()
{
    // io;
    cin >> t;

    while (t--)
    {
        cin >> n >> str;
        //修改次数
        int res = 0;
        //题目保证偶数对，因此我们只需要对每2位进行判断
        //是否相同，若不同则我们的操作数就需要+1
        for (int i = 0; i < n; i += 2)
        {
            if (str[i] != str[i + 1])
                res++;
        }
        //最小段数
        int cnt = dppp();

        

        cout << res << " " << cnt << "\n";
    }

    system("pause");
    return 0;
}
/*
111
12
011110001010
12
111110001010
*/

```