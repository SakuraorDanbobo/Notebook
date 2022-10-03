### Question
The task of this problem is simple: insert a sequence of distinct positive integers into a hash table first. Then try to find another sequence of integer keys from the table and output the average search time (the number of comparisons made to find whether or not the key is in the table). The hash function is defined to be H(key)=key%TSize where TSize is the maximum size of the hash table. Quadratic probing (with positive increments only) is used to solve the collisions.

Note that the table size is better to be prime. If the maximum size given by the user is not prime, you must re-define the table size to be the smallest prime number which is larger than the size given by the user.

### Input Specification:
Each input file contains one test case. For each case, the first line contains 3 positive numbers: MSize, N, and M, which are the user-defined table size, the number of input numbers, and the number of keys to be found, respectively. All the three numbers are no more than 10e4
 . Then N distinct positive integers are given in the next line, followed by M positive integer keys in the next line. All the numbers in a line are separated by a space and are no more than 10e5
 .
### Output Specification:
For each test case, in case it is impossible to insert some number, print in a line X cannot be inserted. where X is the input number. Finally print in a line the average search time for all the M keys, accurate up to 1 decimal place.

### Sample Input:
4 5 4
10 6 4 15 11
11 4 15 2
### Sample Output:
15 cannot be inserted.
2.8
### 题意：
```in
给定一个序列，用正向平方探测法解决哈希冲突，然后给出m个数字，如果这个数字不能够被插入就输出”X cannot be inserted.”，然后输出这m个数字的平均查找时间
```
### 分析
```in
Quadratic probing （二次探测法）也叫平方探测发，即探测间隔为平方数。
用来处理哈希冲突，处理冲突的函数为 H(key)=(key+i*i)%TSize  { i<TSize}
当i==TSize时，则表示无法插入成功

Average Search Time （平均查找时间）
查找的顺序依然是 H(key)=(key+i*i)%TSize，{ i <= TSize } ,i的范围与插入时有别。
若当前位置为查找值或者为空时，查找结束。
(为什么查找为空时也要退出？这是因为该位置为空，那么在之前的插入过程中，一定能空位置插入当前数字，所以若遇见当前位置为空，则表明当前数字插入失败。)
Ps[此处均为正向二次探测。]
```
### 思路
```in
首先需要解决地就是表的大小，若不是素数，则找到最小大于TSize的素数，该值即为TSize。
接着创建TSize大小的数组，用来插入数字x，每次探测x在哈希表中的下表(x+j*j)%TSize,(j为0~TSize-1)，若当前位置可以插入，则插入，若无法插入，则输出无法插入。
最后是计算平均查找时间，每次计算pos = (a + j * j) % TSize，(j为0~TSize)，如果v[pos]处正是a则查找到了，则退出循环，如果v[pos]处不存在数字表示没查找到，那么也要退出循环。每次查找的时候，查找次数+1。最后总查找次数除以查找个数得到平均查找时间然后输出。
```
### 参考代码：
```c++
#include <bits/stdc++.h>
using namespace std;
const int N=1e4+5;
bool isss(int x)
{
    if(x<=1)return false;
    if(x==2 || x==3)return true;
    if(x%6!=1 && x%6!=5)return false;
    
    for(int i =5;i<=sqrt(x);i+=6)
    {
        if(x%i==0 || x%(i+2)==0)return false;
    }
    return true;
}
int vis[N];
int  main()
{
    int cnt,n,m,x;
    cin>>cnt>>n>>m;
    //表的大小若不是素数，寻找最接近的素数大小
    while(!isss(cnt))cnt++;
    
    //平方探测法，在插入时，j应小于表的大小即(j<cnt)
    for(int i=0;i<n;i++)
    {
        cin>>x;
        int j=0;
        for(;j<cnt;j++)
        {
            int pos=(x+j*j)%cnt;
            if(vis[pos]==0)
            {
                vis[pos]=x;
                break;
            }
        }
        //若冲突了cnt-1次，则代表无法插入
        if(j==cnt)cout<<x<<" cannot be inserted.\n";
    }

    
    //平方探测法，在查找时，j应小于等于表的大小即(j<=cnt)
    //记录总的比较次数
    int all=0;
    for(int i=0;i<m;i++)
    {
        cin>>x;
        for(int j=0;j<=cnt;j++)
        {
            int pos=(x+j*j)%cnt;
            all++;
            //找到或者该位置为空则停止。
            if(vis[pos]==x || vis[pos]==0)break;
        }
    }
    printf("%.1f",all*1.0/m);
}

```