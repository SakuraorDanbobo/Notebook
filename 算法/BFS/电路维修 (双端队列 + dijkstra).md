#### Question
达达是来自异世界的魔女，她在漫无目的地四处漂流的时候，遇到了善良的少女翰翰，从而被收留在地球上。翰翰的家里有一辆飞行车。有一天飞行车的电路板突然出现了故障，导致无法启动。电路板的整体结构是一个 R 行 C 列的网格（R,C≤500），如下图所示。

![[Pasted image 20220501214318.png]]

每个格点都是电线的接点，每个格子都包含一个电子元件。

电子元件的主要部分是一个可旋转的、连接一条对角线上的两个接点的短电缆。

在旋转之后，它就可以连接另一条对角线的两个接点。

电路板左上角的接点接入直流电源，右下角的接点接入飞行车的发动装置。

达达发现因为某些元件的方向不小心发生了改变，电路板可能处于断路的状态。

她准备通过计算，旋转最少数量的元件，使电源与发动装置通过若干条短缆相连。

不过，电路的规模实在是太大了，达达并不擅长编程，希望你能够帮她解决这个问题。

**注意**：只能走斜向的线段，水平和竖直线段不能走。

#### 输入格式

输入文件包含多组测试数据。

第一行包含一个整数 T，表示测试数据的数目。

对于每组测试数据，第一行包含正整数 R 和 C，表示电路板的行数和列数。

之后 R 行，每行 C 个字符，字符是`"/"`和`"\"`中的一个，表示标准件的方向。

#### 输出格式

对于每组测试数据，在单独的一行输出一个正整数，表示所需的最小旋转次数。

如果无论怎样都不能使得电源和发动机之间连通，输出 `NO SOLUTION`。

#### 数据范围

1≤R,C≤500,  
1≤T≤5

#### 输入样例：

```
1
3 5
\\/\\
\\///
/\\\\
```

#### 输出样例：

```
1
```

#### 样例解释

样例的输入对应于题目描述中的情况。
只需要按照下面的方式旋转标准件，就可以使得电源和发动机之间连通。
![[Pasted image 20220501214244.png]]


#### 思路：
    首先，点跟点之间的距离只为0或者1，因此可以用堆优化的dijkstra。但是因为每次增长量只为1或者0这2种情况，因此可以用双端队列来替代优先队列，当增量为0时，放在队头；当增量为1时，放在队尾。这样可以保证每次更新的路径总是最优的。
	
    其次，注意下点方向的处理即可。
    3个数组只要按顺序处理。这样的话方便处理。
    1.dir数组，当前点的下一个4个方向路线。
    2.cd数组，下一个点的路线状态
    3.pc，下一个点花费为0的路线状态

#### 参考代码：
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long
#define x first
#define y second
#define PII pair<int,int>
#define PIII pair<int,PII>
#define PIIS pair<int,pair<int,string> >
#define PDD pair<double,double>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=3e5;
const int M=1e7;
const int mod=1e9+7;

int n,m,t;
int dis[555][555];
char ma[555][555];
/*
    思路：
    首先，点跟点之间的距离只为0或者1，因此可以用堆优化的dijkstra。但是因为
    每次增长量只为1或者0这2种情况，因此可以用双端队列来替代优先队列，当增量为0
    时，放在队头；当增量为1时，放在队尾。这样可以保证每次更新的路径总是最优的。
    其次，注意下点方向的处理即可。
    3个数组只要按顺序处理。这样的话方便处理。
    1.dir数组，当前点的下一个4个方向路线。
    2.cd数组，下一个点的路线状态
    3.pc，下一个点花费为0的路线状态
*/
void bfs()
{
    deque<PII>dq;
    memset(dis,INF,sizeof dis);
    //左上，右上，右下，左下,下一点位置
    int dir[4][2]={-1,-1,-1,1,1,1,1,-1};
    //出发点路线位置
    int cd[4][2]={-1,-1,-1,0,0,0,0,-1};
    //左上，右上，右下，左下 为0的路线
    char pc[]="\\/\\/";

    dis[0][0]=0;
    dq.push_back({0,0});

    while(dq.size())
    {
        PII tmp=dq.front();
        dq.pop_front();
        int x=tmp.x;
        int y=tmp.y;

        for(int i=0,nx,ny,dx,dy;i<4;i++)
        {
            nx=x+dir[i][0];
            ny=y+dir[i][1];
            if(nx<0 || nx>n || ny<0 || ny>m)continue;
            
            dx=x+cd[i][0];
            dy=y+cd[i][1];
            int add=0;
            if(ma[dx][dy]!=pc[i])add++;
            if(dis[nx][ny]>dis[x][y]+add)
            {
                dis[nx][ny]=dis[x][y]+add;
                //需要旋转，则将本次点添加到队尾
                if(add)dq.push_back({nx,ny});
                //不需要旋转，则将本次点添加到队头
                else dq.push_front({nx,ny});
            }
        }

    }

    if(dis[n][m]!=INF)printf("%d\n",dis[n][m]); 
    else printf("NO SOLUTION\n");
}

int main()
{
    // io;
    
    // while(~scanf("%d",&t))
    // {
        scanf("%d",&t);
        while(t--)
        {
            scanf("%d%d",&n,&m);
            for(int i=0;i<n;i++)scanf("%s",ma[i]);
            bfs();
        }
    // }
    // system("pause");
    return 0;
} 
/*
4
3 3
//\
/\/
\/\
3 3
///
//\
///
2 4
\\\\
\\\\
3 3
\\\
/\/
\//
*/
```