描述
对于任何正整数 x，其约数的个数记作 g(x)。例如 g(1)=1，g(6)=4。

如果某个正整数 x 满足：∀0<i<x，都有 g(x)>g(i)，则称 x 为 Antiprime数。例如，整数 1,2,4,6,12,24 都是Antiprime数。

输入
输入一行为一个整数N，1<=n<=2*10e9

输出
输出一行一个整数，即不大于n的最大Antiprime数。

样例输入
1000
样例输出

840



```java
import java.util.Scanner;

/**
 * @ClassName: Main
 * @Description:
 * @author: Danbobo
 * @date: 2022/3/28 15:49
 */
public class Main
{
    static Scanner sz=new Scanner(System.in);
    static long n,cnum,csum;
    static int[] prim=new int[]{2,3,5,7,11,13,17,19,21,23};
    /*
     题意：求[1..N]中最大的反素数,即求约数最多的数（约数同样多取数值小的）
     那么约数的个数如何求呢？
     例， 24=2^3*3  , (2^0+2^1+2^2+2^3)*(3^0+3^1) <==>(3+1)*(1+1)=8(种)
     因此我们按质因子从小到大的顺序枚举每一个质因子，
     因为2*3*5*7*11*13*17*19*21*23>n，所以只需考虑到23这个质因子即可。
     性质一:一个反素数的质因子必然是从2开始连续的质数.
     性质二:p=2^t1*3^t2*5^t3*7^t4.....必然t1>=t2>=t3>=....
    */
    public static void main(String[] args)
    {
        n=sz.nextLong();
        solve(1,1,10,0);
        System.out.println(csum);
    }

    //组成num这个数时，num的约数个数为sum个。第cnt个素数，个数最多为limit个
    private static void solve(long num,long sum,int limit,int cnt)
    {
        //更新约数更多的数
        if(num>cnum)
        {
            csum=sum;
            cnum=num;
        }
        //约数个数一样时，取数小的
        else if(num==cnum && sum<csum)
        {
            csum=sum;
        }

        //第cnt个素数取i个
        for(int i=1;i<=limit;i++)
        {
            sum*= prim[cnt];
            if(sum>n)return;
            solve(num*(1+i),sum,limit,cnt+1);
        }
    }


}

```

