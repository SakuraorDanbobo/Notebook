描述

Compared to wildleopard's wealthiness, his brother mildleopard is rather poor. His house is narrow and he has only one light bulb in his house. Every night, he is wandering in his incommodious house, thinking of how to earn more money. One day, he found that the length of his shadow was changing from time to time while walking between the light bulb and the wall of his house. A sudden thought ran through his mind and he wanted to know the maximum length of his shadow.


输入

The first line of the input contains an integer T (T <= 100), indicating the number of cases.

Each test case contains three real numbers H, h and D in one line. H is the height of the light bulb while h is the height of mildleopard. D is distance between the light bulb and the wall. All numbers are in range from 10e-2 to 10e3, both inclusive, and H - h >= 10e-2.

输出

For each test case, output the maximum length of mildleopard's shadow in one line, accurate up to three decimal places..

样例输入

3
2 1 0.5
2 0.5 3
4 3 4


样例输出

1.000
0.750
4.000

 

```c++
#include <bits/stdc++.h>

using namespace std;
const double eps=1e-6;

double H,h,D;
//x为人距离灯泡的距离 
double get(double x)
{
	//水平距离(D-x) ， 垂直距离 H-(H-h)*D/x
	return (D-x)+(H-(H-h)*D/x);
}
//三分法, 枚举人距离灯泡距离的值 
void three_get()
{
	double l=D-h*D/H,r=D;
	double lmid,rmid;
	while(l+eps<r)
	{
		lmid=(l+r)/2;
		rmid=(lmid+r)/2;
		if(get(lmid)<get(rmid))
		{
			l=lmid;
		}
		else r=rmid;
	}
	printf("%.3lf\n",get(l));
} 

//公式法 
void solve()
{
	double x0,x;
	x0=D-h*D/H; //灯，人，墙角共线时，人所在的位置 
	//  (a*b)/x + x >= 2*sqrt(a*b)  
	x=sqrt(D*(H-h));
	if(x>=D)printf("%.3lf\n",h);//人靠墙，影子完全在墙上， 
	else if(x<x0)printf("%.3lf\n",h*D/H); //没有影子在墙上时 
	else printf("%.3lf\n",D+H-2*x); //影子一段在地上，一段在墙上 
}
int main()
{
	int t;
	cin>>t;
	while(t--)
	{
		cin>>H>>h>>D;
		three_get();
//		solve();
	}
} 
```

