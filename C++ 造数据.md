#### rand()
- 随机数大小是在0到RAND_MAX，值为2147483647，它是在stdlib中定义的，如果我们希望在某个范围内，可以使用 % 结合 / 来实现。
- 例如：rand() % 100 + 1 ，[1,100]间的数字。

#### 构造数据
```c++
#include <bits/stdc++.h>
using namespace std;
int main()
{
	char* file = "a.in";
	//所有当前输出会保存在当前文件夹下的a.in文件下
    freopen(file,"w",stdout);
	//把当前时间作为随机数种子，没有这一步你的随机是假的
	srand(time(NULL));
	int a,b;
	a=rand();
	b=1;
	cout<<a<<" "<<b<<"\n";

	return 0;

}

```

#### 得到标程
```c++
表示以data1.in作为输入流，对文件进行读取
freopen("data1.in","r",stdin); 
以data1.out作为输出流，对文件进行写入freopen("data1.out","w",stdout);
```