**描述**

给定两个正整数A和B（0<A, B<=5*1e7），求A^B的所有约数之和，因为结果可能很大，你只要将结果对9901取余即可。

**输入**

两个正整数A和B（0<A, B<=5*1e7）

**输出**

A^B 约数之和mod 9901。

**样例输入**

2 3

**样例输出**

15

 ```c+
 #include <bits/stdc++.h>
 #define IO ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
 using namespace std;
 typedef long long LL;
 typedef unsigned long long ull;
 const int INF=0x3f3f3f3f;
 const int N=1e5;
 const int mod=9901;
 /*
 1.整数的唯一分解定理：
 任意正整数都有且只有一种方式写出其素因子的乘积表达式。
 A=(p1^k1)*(p2^k2)*(p3^k3)*....*(pn^kn)   其中pi均为素数
 
 2.约数和公式：
 对于已经分解的整数A=(p1^k1)*(p2^k2)*(p3^k3)*....*(pn^kn)
 有A的所有因子之和为
  S = (1+p1+p1^2+p1^3+...p1^k1) * (1+p2+p2^2+p2^3+….p2^k2) * (1+p3+ p3^3+…+ p3^k3) * .... * (1+pn+pn^2+pn^3+...pn^kn)
  
 3.同余模公式：
 (a+b)%m=(a%m+b%m)%m
 (a*b)%m=(a%m*b%m)%m
 */
 
 int p[N],k[N]; //A的分解式,p[i]^k[i]
 int cnt; //记录个数
 void divide(int n) //对n进行素因子分解
 {
     cnt=0;
     //n本身不是质数的情况下
     for(int i=2;i*i<=n;)
     {
         if(n%i==0)
         {
             p[cnt]=i;
             while(n%i==0) 
             {
                 k[cnt]++;
                 n/=i;
             }
             cnt++;
         }
         //素因子2除完后就无偶数情况，因此都跳过，判断奇数的情况
         if(i==2)i++; 
         else i+=2;
     }
     //循环结束时n不等于1，即n仍为质数，记录下来
     if(n!=1)
     {
         p[cnt]=n;
         k[cnt++]=1;
     }
     
 }
 LL quick_pow(LL a,LL b) //快速幂
 {
     LL ans=1;
     while(b)
     {
         if(b&1)ans=ans*a%mod;
         a=a*a%mod;
         b>>=1;
     }
     return ans;
 }
 /*
  递归二分求 (1 + p + p^2 + p^3 +...+ p^n)%mod
  奇数二分式 (1 + p + p^2 +...+ p^(n/2)) * (1 + p^(n/2+1))
  偶数二分式 (1 + p + p^2 +...+ p^(n/2-1)) * (1+p^(n/2+1)) + p^(n/2)
 */
 LL sum(LL a,LL b)
 {
     if(b==0)return 1;
     if(b&1)//奇数次
     {
         return (sum(a,b/2)*(1+quick_pow(a,b/2+1)))%mod;
     }
     else //偶数次
     {
         return (sum(a,b/2-1)*(1+quick_pow(a,b/2+1))+quick_pow(a,b/2))%mod;
     }
 }
 int main()
 {
 	IO;
 	int a,b;
     cin>>a>>b;
     
     divide(a);
     int ans=1; //约数和
     // cout<<cnt<<"\n";
     // cout<<p[0]<<" "<<k[0]<<"\n";
     for(int i=0;i<cnt;i++)
     {
         // cout<<sum(p[i],k[i]*b)<<"\n";
         ans=ans*(sum(p[i],k[i]*b)%mod)%mod;
     }
     cout<<ans;
 } 
 ```

