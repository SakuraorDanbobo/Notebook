描述

对于给定的一个长度为N的正整数数列A[i]，现要将其分成M（M≤N）段，并要求每段连续，且每段和的最大值最小。

关于最大值最小：

例如一数列4 2 4 5 1要分成3段

将其如下分段：

```
[4 2][4 5][1]
```

第一段和为6，第2段和为9，第3段和为1，和最大值为9。

将其如下分段：

```
[4][2 4][5 1]
```

第一段和为4，第2段和为6，第3段和为6，和最大值为6。

并且无论如何分段，最大值不会小于6。

所以可以得到要将数列4 2 4 5 1要分成3段，每段和的最大值最小为6。

输入

第1行包含两个正整数N，M，第2行包含N个空格隔开的非负整数A[i]，含义如题目所述。

M<=N<=100000, A[i]之和不超过109

输出

仅包含一个正整数，即每段和最大值最小为多少。

样例输入

5 3
4 2 4 5 1


样例输出

6

 

思路： 二分枚举每段和的最值

```java
import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.StreamTokenizer;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Scanner;
import java.util.Set;
import java.util.Stack;
import java.util.TreeSet;

public class Main 
{
	static Scanner sz=new Scanner(new BufferedInputStream(System.in));
	static StreamTokenizer in=new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in),32768));
	static PrintWriter out=new PrintWriter(new OutputStreamWriter(System.out));
	
	static final int N=(int)(1e5+5);
	static int n,m,f[];
	public static void main(String[] args) 
	{
		n=sz.nextInt();
		m=sz.nextInt();
		f=new int[n+1];
		int l=0,r=0;
		for(int i=1;i<=n;i++)
		{
			f[i]=sz.nextInt();
			//右边界取所有数和
			r+=f[i];
			//左边界去取单个最大值
			l=Math.max(l,f[i]);
		}
		//二分枚举每段和的最大值
		while(l<=r)
		{
			int mid=(l+r)>>1;
//			System.out.print("l: "+l+"  r: "+r+" | mid:"+mid+" ^^");
			//最大值尽可能小，因此不断收缩右边界
			if(pd(mid))
			{
				r=mid-1;
			}
			else l=mid+1;
		}
		System.out.println(l);
	}
	private static boolean pd(int ans) 
	{
		//最大值为ans时能划分的k段
		int k=1;
		int cur=0;
		for(int i=1;i<=n;i++)
		{
			//当左边界没取单个最大值时需要特判
//			if(f[i]>ans)return false;
			if(cur+f[i]>ans)
			{
//				System.out.print(cur+" ");
				cur=f[i];
				k++;
			}
			else cur+=f[i];
		}
//		System.out.println(" | k:"+k+" ans:"+ans);
		if(k<=m)return true;
		else return false;
	}
}
/*
*/
 
```

