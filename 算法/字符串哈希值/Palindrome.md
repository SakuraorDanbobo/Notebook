描述

Given a string consisting of some digits,  please find out the maximal k to split it into k parts of consecutive substrings, these k parts then become palindrome.

For example, given the string "652526", we can split it into 4 parts: 6|52|52|6, then part 1 equals part 4 (6), and part 2 equals part 3 (52).

输入

Input a string (no more than 106 digits).

输出

Output an integer, the maximal value of k.

样例输入

652526


样例输出

4

Sample Input 2:

332333

Sample Output 2:

5

 

思路： 预处理哈希字符串，保证O（1）的时间来比较。接着就从前后往中间比较即可。

```java
import java.util.*;

/**
 * @ClassName: Main
 * @Description: None
 * @author: Danbobo
 * @date: 2022/3/3 15:05
 */
public class Main
{
    static long mod= (long) (1e9+7);
    static  int P=131313;
    static Scanner sz=new Scanner(System.in);
    static long hash[],p[];
    public static void main(String[] args)
    {
        String str=" "+sz.next();
        int n=str.length();
        hash=new long[n];
        p=new long[n];
        p[0]=1;
        for(int i=1;i<n;i++)
        {
            p[i]=p[i-1]*P%mod;
            hash[i]=(hash[i-1]*P+(str.charAt(i)-'0'+1))%mod;
        }

        int l=1,r=n-1;
        long ans=0;
        int k=1;
        //652526 12512
        while(l<r)
        {
            while(l+k-1<r-k+1)
            {
                if(query(l,l+k-1)==query(r-k+1,r))
                {
                    l+=k;
                    r-=k;
                    ans+=2;
                    k=1;
                    break;
                }
                k++;
            }
            if(l+k-1>=r-k+1)break;
        }
        //剩下无法分割都成一块
        if(l<=r)ans++;
        System.out.println(ans);

    }
    //哈希前缀查询
    static long query(int l,int r)
    {
        return (hash[r]-hash[l-1]*p[r-l+1]%mod+mod)%mod;
    }
}
/*
332333
*/

```

