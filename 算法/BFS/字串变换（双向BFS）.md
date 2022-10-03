#### 问题描述:
已知有两个字串 A, B 及一组字串变换的规则（至多 6 个规则）:

A1→B1

A2→B2

…

规则的含义为：在 A 中的子串 A1 可以变换为 B1、A2 可以变换为 B2…。

例如：A＝`abcd` B＝`xyz`

变换规则为：

`abc` → `xu` , `ud` → `y` ,`y` → `yz`

则此时，A 可以经过一系列的变换变为 B，其变换的过程为：

`abcd` → `xud` → `xy` → `xyz`

共进行了三次变换，使得 A 变换为 B。

#### 输入格式

输入格式如下：

A B  
A1 B1  
A2 B2  
…

第一行是两个给定的字符串 A 和 B。

接下来若干行，每行描述一组字串变换的规则。

所有字符串长度的上限为 20。

#### 输出格式

若在 10 步（包含 10 步）以内能将 A 变换为 B ，则输出最少的变换步数；否则输出 `NO ANSWER!`。

#### 输入样例：

```in
abcd xyz
abc xu
ud y
y yz
```

#### 输出样例：

```out
3
```

#### 思路：
```in
  使用双向bfs,从起点正向搜索和终点逆向搜索，这样能有效减少枚举的宽度。
  当两边搜索到相同的值，意味着找到了一条联通起点和终点的最短路径。
注意: 1.每次应该扩一层，而不是一个点。 
     2.扩展的时候，我们可以从数据较少的一侧开始扩展。
     3.如果有一个队列为空则退出循环，即A状态无法到达B状态。
```
#### 参考代码：
```c++
#include <bits/stdc++.h>

using namespace std;
/*
*/ 
string st,ed;
int n;
string a[7],b[7];
//扩展点 
int extend(queue<string> &q,map<string,int> &dl,map<string,int> &dr,string l[],string r[])
{
    //记录当前层
    int cur= dl[q.front()];
    while(q.size() && dl[q.front()]==cur)
    {
        string str=q.front();
        q.pop();
//      cout<<str<<" "<<dl[str]<<"-------\n";
        int len=str.length();
        //枚举操作 
        for(int i=0;i<n;i++)
        {
            int lc=l[i].size();
            //枚举当前字符串 
            for(int j=0;j<=len-lc;j++)
            {
                //满足替换要求 
                if(str.substr(j,lc)==l[i])
                {
                    //前半部分+替换部分+后半部分 
                    string s=str.substr(0,j)+r[i]+str.substr(j+lc);
                    if(dr.count(s))return dl[str]+dr[s]+1;
                    if(dl.count(s))continue;
                    dl[s]=dl[str]+1;
                    q.push(s);
                }
            } 
         } 

    }
    //返回一个不可能到达的步数 
    return 11;
}
int bfs()
{
    //特判开始 
    if(st==ed)return 0;

    //起点和终点的搜索队列 
    queue<string> l,r;
    //每个状态的拓展步数 
    map<string,int> dl,dr;
    l.push(st);
    dl[st]=0;
    r.push(ed);
    dr[ed]=0;

    int step=0;
    //从决策少的一侧出发 
    while(l.size() && r.size())
    {
        int t;
        if(l.size()<=r.size()) t=extend(l,dl,dr,a,b); 
        else t=extend(r,dr,dl,b,a); 
        if(t<=10)return t;
        if(++step==10)return -1;
    } 
    return -1; 
}
int main()
{
    cin>>st>>ed;
    //读入变换规则 
    while(cin>>a[n]>>b[n])
    {
        n++;
    }
    int ans=bfs();
    if(ans==-1)cout<<"NO ANSWER!\n";
    else cout<<ans<<"\n";
    return 0;
}
```
