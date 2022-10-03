给定一个长度为N的数列，A1, A2, ... AN，如果其中一段连续的子序列Ai, Ai+1, ... Aj(i <= j)之和是K的倍数，我们就称这个区间[i, j]是K倍区间。  

你能求出数列中总共有多少个K倍区间吗？  

输入
-----
第一行包含两个整数N和K。(1 <= N, K <= 100000)  
以下N行每行包含一个整数Ai。(1 <= Ai <= 100000)  

输出
-----
输出一个整数，代表K倍区间的数目。  


输入：
5 2
1 
2 
3 
4 
5  

输出：
6



```java
import java.util.*;
public class Main
{
    static Scanner sz=new Scanner(System.in);

    public static void main(String []args) throws Exception
    {
        int n=sz.nextInt();
        int k=sz.nextInt();
        int res[]=new int[100005];
        long sum=0;
        long ans=0;
        res[0]=1;
        // (S[r]-S[l-1]) %k=0  ==> S[r]%k=S[l-r]%k
        while(n-->0)
        {
            int x=sz.nextInt();
            sum+=x;
            ans+=res[(int) (sum%k)];
            res[(int) (sum%k)]++;
        }
        System.out.println(ans);
    }
}
/*
5 2
1 2 3 4 5
*/
```

