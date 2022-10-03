```in
定义：
    欧拉函数phi(n)是数论中非常重要的一个函数，其表示1到n-1之间，与n互质的数的个数。
    phi(n)=n(1-(1/p1))(1-(1/p2))…(1-(1/pk))
    稍稍化简一下就是
    phi(n)=n(p1-1)(p2-1)…(pk-1)/(p1p2…*pk)

思路：
    利用分解质因数的思路
    依次遍历当可以整除质数存储答案
    判断当不能除后最后一个数的大小
    
性质：
① 当m,n互质时，有phi（m*n）= phi（m）*phi（n）；

② 若i%p==0，有phi（i*p） = p * phi(i)；

③ 对于互质x与p，有x^phi§≡1（mod p),因此x的逆元为x^(phi§-1)，即欧拉定理。
（特别地，当p为质数时，phi（p）=p-1,此时逆元为x^(p-2)，即费马小定理）

④ 当n为奇数时，phi(2n)=phi(n)

⑤ 若x与p互质，则p-x也与p互质，因此小于p且与p互质的数之和为phi(x)*x/2;

⑥N>1，不大于N且和N互素的所有正整数的和是 1/2 *N *eular(N)。

⑦若(N%a == 0 && (N/a)%a==0) 则有:E(N)=E(N/a)*a;

⑧若(N%a==0 && (N/a)%a!=0) 则有:E(N)=E(N/a)*(a-1);
 
```



求某个数的欧拉函数

```c++
ll eular(ll n)
{
    ll ans = n;
    for(int i=2; i*i <= n; ++i)
    {
        //如果整除素数
        if(n%i == 0)  
        {
            //存入答案将n*(1-(1/p))转化为n/p*(p-1) 
            ans = ans/i*(i-1);
            while(n%i == 0)
                n/=i;
        }
    }
    //当除到最后大于1说明还剩余一个素因子
    if(n > 1) ans = ans/n*(n-1);
    return ans;
}
```

欧拉筛素数的同时求欧拉函数

```c++
void get_phi()  
{  
    int i, j, k;  
    k = 0;  
    for(i = 2; i < maxn; i++)  
    {  
        if(is_prime[i] == false)  
        {  
            prime[k++] = i;  
            phi[i] = i-1;  
        }  
        for(j = 0; j<k && i*prime[j]<maxn; j++)  
        {  
            is_prime[ i*prime[j] ] = true;  
            if(i%prime[j] == 0)  
            {  
                phi[ i*prime[j] ] = phi[i] * prime[j];  
                break;  
            }  
            else  
            {  
                phi[ i*prime[j] ] = phi[i] * (prime[j]-1);  
            }  
        }  
    }  
}  
 
```

