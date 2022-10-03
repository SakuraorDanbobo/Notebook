**描述**

推箱子是一个很经典的游戏.今天我们来玩一个简单版本.在一个M*N的房间里有一个箱子和一个搬运工,搬运工的工作就是把箱子推到指定的位置,注意,搬运工只能推箱子而不能拉箱子,因此如果箱子被推到一个角上(如图2)那么箱子就不能再被移动了,如果箱子被推到一面墙上,那么箱子只能沿着墙移动.  
  
现在给定房间的结构,箱子的位置,搬运工的位置和箱子要被推去的位置,请你计算出搬运工至少要推动箱子多少格.  
 

![[Pasted image 20220502203028.png]] 

**输入**

输入数据的第一行是一个整数T(1<=T<=20),代表测试数据的数量.然后是T组测试数据,每组测试数据的第一行是两个正整数M,N(2<=M,N<=7),代表房间的大小,然后是一个M行N列的矩阵,代表房间的布局,其中0代表空的地板,1代表墙,2代表箱子的起始位置,3代表箱子要被推去的位置,4代表搬运工的起始位置.

**输出**

对于每组测试数据,输出搬运工最少需要推动箱子多少格才能帮箱子推到指定位置,如果不能推到指定位置则输出-1.

**样例输入**
1
5 5
0 3 0 0 0
1 0 1 4 0
0 0 1 0 0
1 0 2 0 0
0 0 0 0 0

**样例输出**
4


```c++

#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define LL long long
#define PII pair<int,int>
#define PIII pair<int,PII>
#define PSI pair<string,int>
#define PIIS pair<int,pair<int,string> >
#define PDD pair<double,double>
using namespace std;
const int INF=0x3f3f3f3f;
const int N=3e5;
const int M=1e7;
const int mod=1e9+7;

struct node
{
    int x,y,dir,cnt;
};
int n,m,t;
int sx,sy,ex,ey,bx,by;
int ma[11][11];
//上下左右
int dir[4][2]={-1,0,1,0,0,-1,0,1};

bool check(int x,int y)
{
    if(x<1 || x>n || y<1 || y>m || ma[x][y]==1)return false;
    return true;
}
bool vpeo[11][11];
// a,b 人当前坐标， {c,d}箱子坐标，{e,f}人需要到达的终点
bool bfs_peo(int a,int b,int c,int d,int e ,int f)
{
    queue<PII>q;
    memset(vpeo,0,sizeof vpeo);
    q.push({a,b});
    vpeo[a][b]=1;
    vpeo[c][d]=1;
    while(q.size())
    {
        PII tmp=q.front();
        q.pop();
        if(tmp.first==e && tmp.second==f)return true;

        for(int i=0;i<4;i++)
        {
            int nx=tmp.first+dir[i][0];
            int ny=tmp.second+dir[i][1];

            if(!check(nx,ny) || vpeo[nx][ny])continue;

            vpeo[nx][ny]=1;
            q.push({nx,ny});
        }
    }

    return false;
}

//判断箱子从4个方向是否访问过
bool vbox[11][11][4];
int bfs_box()
{
    queue<node> q;
    memset(vbox,0,sizeof(vbox));
    //先初始推动1次判断状态
    int nx,ny,px,py;
    for(int i=0;i<4;i++)
    {
        nx=bx+dir[i][0];
        ny=by+dir[i][1];

        px=bx-dir[i][0];
        py=by-dir[i][1];
        
        // cout<<"人开始位置:"<<sx<<" "<<sy<<" 人终点位置"<<px<<" "<<py<<" "<<bfs_peo(sx,sy,bx,by,px,py)<<"---\n";
        if(check(nx,ny) && check(px,py) && bfs_peo(sx,sy,bx,by,px,py))
        {
            // cout<<"箱子下一个点的位置: "<<nx<<" "<<ny<<" | 人要到达的位置"<<px<<" "<<py<<"----\n";
            vbox[nx][ny][i]=1;
            q.push({nx,ny,i,1});
        }
    }
    // cout<<q.size()<<"\n";
    int ans=INF;
    while(q.size())
    {
        node tmp=q.front();
        q.pop();
        // cout<<tmp.x<<" "<<tmp.y<<" "<<tmp.dir<<"---\n";
        if(tmp.x==ex && tmp.y==ey)
        {
            ans=min(ans,tmp.cnt);
            continue;
        }
        

        for(int i=0;i<4;i++)
        {
            //箱子要去的点
            nx=tmp.x+dir[i][0];
            ny=tmp.y+dir[i][1];
            
            //人需要达到的点
            px=tmp.x-dir[i][0];
            py=tmp.y-dir[i][1];

            //人当前所在点
            int xx=tmp.x-dir[tmp.dir][0];
            int yy=tmp.y-dir[tmp.dir][1];

            if(check(nx,ny) && check(px,py) && bfs_peo(xx,yy,tmp.x,tmp.y,px,py) && !vbox[nx][ny][i])
            {
                vbox[nx][ny][i]=1;
                q.push({nx,ny,i,tmp.cnt+1});
            }
        }
    }
    return ans==INF?-1:ans;
}
int main()
{
    io;
    cin>>t;
    while(t--)
    {
        cin>>n>>m;
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=m;j++)
            {
                cin>>ma[i][j];
                if(ma[i][j]==2)
                {
                    bx=i;
                    by=j;
                }
                if(ma[i][j]==3)
                {
                    ex=i;
                    ey=j;
                }
                if(ma[i][j]==4)
                {
                    sx=i;
                    sy=j;
                }
            }
        }
        cout<<bfs_box()<<"\n";
    }
    // system("pause");
    return 0;
} 
/*
1
5 5
0 3 0 0 0
1 0 1 4 0
0 0 1 0 0
1 0 2 0 0
0 0 1 0 0
*/
```