**描述**

Farmer John's farm consists of a long row of N (1 <= N <= 100,000)fields. Each field contains a certain number of cows, 1 <= ncows <= 2000.  
  
FJ wants to build a fence around a contiguous group of these fields in order to maximize the average number of cows per field within that block. The block must contain at least F (1 <= F <= N) fields, where F given as input.  
  
Calculate the fence placement that maximizes the average, given the constraint.  

**输入**

* Line 1: Two space-separated integers, N and F.  
  
* Lines 2..N+1: Each line contains a single integer, the number of cows in a field. Line 2 gives the number of cows in field 1,line 3 gives the number in field 2, and so on.

**输出**

* Line 1: A single integer that is 1000 times the maximal average.Do not perform rounding, just print the integer that is 1000*ncows/nfields.

**样例输入**
10 6
6 
4
2
10
3
8
5
9
4
1
**样例输出**
6500

**思路**:
```
	1.对于二分，二分是二分性而不是单调性 只要满足可以找到一个值一半满足一半不满足即可 而不用满足单调性

    2.因此，对于本题，我们二分枚举区间个数不小于f的区间和的平均数。以下注意点：

    a.对于一段区间，每个数减去区间的平均数，如果大于0 那么他本身就大于平均数，如果小于0 那么它本身就小于平均数。

    我们直接用前缀和来统计每个区间，来达到快速判断一个区间里的平均值是否大于或小于我们二分枚举的平均数。

    b.若我们枚举长度至少为f的区间最优时，即[l,r]，那么就是保证a[l−1]要尽量地小，然后a[r]要尽量地大，所以说

    我们就需要枚举这个l，但是这样的话时间复杂度就上去了。我们发现，每一次r变大后，l的取值范围从[1,l]变成了[1,l+1]，

    因此我们定义一个minc去每次存储l变化后[1,l]的最优极小值

    c.最后结果向下取整，因此我们取的是r，而不是l。若是取l，可能会因为精度差，达不到结果。
```

**参考代码**

```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long

#define x first

#define y second

#define PII pair<int,int>

#define PIS pair<int,string>

#define PIII pair<int,PII>

#define PDD pair<double,double>

using namespace std;

const int INF=0x3f3f3f3f;

const int N=1e5+5;

const int M=1e7;

const int mod=1e9+7;

  

int n,m,f;

int c[N];

double sum[N];

/*


*/

bool ok(double avg)

{

    //将每个地牛的个数减去平均值，通过前缀和来判断一个区间是否大于这个平均数

    //当前缀和小于0时，即区间平均数小于这个平均值

    //当前缀和大于0时，即区间平均数大于这个平均值

    for(int i=1;i<=n;i++)sum[i]=sum[i-1]+c[i]-avg;

    //设置最小值

    double minc=0;

    for(int i=0,j=f;j<=n;j++,i++)

    {

        //存储1~l的最优极小值

        minc=min(minc,sum[i]);

        //满足条件，返回

        if(sum[j]>=minc)return true;

    }

    //都不满足该平均数

    return false;

}

int main()

{

    io;

    cin>>n>>f;

    for(int i=1;i<=n;i++)

    {

        cin>>c[i];

    }

  

    double l=0,r=2000;

    //二分区间的最大平均数 mid

    while(r-l>1e-5)

    {

        double mid=(l+r)/2;

        if(ok(mid))l=mid;

        else r=mid;

    }

    cout<<int(r*1000);

  

    // system("pause");

    return 0;

}
```