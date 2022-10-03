阿兰是某机密部门的打字员，她现在接到一个任务：需要在一天之内输入几百个长度固定为 6 的密码。

当然，她希望输入的过程中敲击键盘的总次数越少越好。

不幸的是，出于保密的需要，该部门用于输入密码的键盘是特殊设计的，键盘上没有数字键，而只有以下六个键：`Swap0, Swap1, Up, Down, Left, Right`。

为了说明这 6 个键的作用，我们先定义录入区的 6 个位置的编号，从左至右依次为 1,2,3,4,5,6。

下面列出每个键的作用：

-   `Swap0`：按 `Swap0`，光标位置不变，将光标所在位置的数字与录入区的 1 号位置的数字（左起第一个数字）交换。如果光标已经处在录入区的 1 号位置，则按 `Swap0` 键之后，录入区的数字不变；
-   `Swap1`：按 `Swap1`，光标位置不变，将光标所在位置的数字与录入区的 6 号位置的数字（左起第六个数字）交换。如果光标已经处在录入区的 6 号位置，则按 `Swap1` 键之后，录入区的数字不变；
-   `Up`：按 `Up`，光标位置不变，将光标所在位置的数字加 1（除非该数字是 9）。例如，如果光标所在位置的数字为 2，按 `Up` 之后，该处的数字变为 3；如果该处数字为 9，则按 `Up` 之后，数字不变，光标位置也不变；
-   `Down`：按 `Down`，光标位置不变，将光标所在位置的数字减 1（除非该数字是 0），如果该处数字为 0，则按 `Down` 之后，数字不变，光标位置也不变；
-   `Left`：按 `Left`，光标左移一个位置，如果光标已经在录入区的 1 号位置（左起第一个位置）上，则光标不动；
-   `Right`：按 `Right`，光标右移一个位置，如果光标已经在录入区的 6 号位置（左起第六个位置）上，则光标不动。

当然，为了使这样的键盘发挥作用，每次录入密码之前，录入区总会随机出现一个长度为 6 的初始密码，而且光标固定出现在 1 号位置上。

当巧妙地使用上述六个特殊键之后，可以得到目标密码，这时光标允许停在任何一个位置。

现在，阿兰需要你的帮助，编写一个程序，求出录入一个密码需要的最少的击键次数。

#### 输入格式

共一行，含有两个长度为 6 的数，前者为初始密码，后者为目标密码，两个密码之间用一个空格隔开。

#### 输出格式

共一行，含有一个正整数，为最少需要的击键次数。

#### 输入样例：

```in
123456 654321
```

#### 输出样例：

```out
11
```

#### 思路：
```in
 6种操作方式，注意下左移和右移操作即可。
```

#### 参考代码：
```c++
#include <bits/stdc++.h>
#define io ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define LL long long
#define PII pair<int, int>
#define PIII pair<int, PII>
#define PSI pair<string, int>
#define PIIS pair<int, pair<int, string>>
#define PDD pair<double, double>
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 1e7 + 5;
const int M = 1e6;
const int mod = 1e9 + 7;
const int add = 1e6;
int st, ed;
bool vis[N][7];
int pos[6] = {100000, 10000, 1000, 100, 10, 1};
int bfs()
{
    int l, r, res;
    int nex;
    queue<PII> q;
    //前置1位+6位密码+1位位置，防止头部出现0的情况
    st = st * 10 + 0;
    q.push({st, 0});
    while (q.size())
    {
        PII tmp = q.front();
        q.pop();
        int ma = tmp.first / 10;
        int dir = tmp.first % 10;
        if (ma == ed)
        {
            return tmp.second;
        }

        //与第0位数字交换
        if (dir != 0)
        {
            // 0位数字
            l = ma / pos[0] % 10;
            //获取当前位
            r = ma / pos[dir] % 10;
            res = ma - l * pos[0] + r * pos[0] - r * pos[dir] + l * pos[dir];
            if (!vis[res][dir])
            {
                if(res==ed)return tmp.second+1;
                vis[res][dir] = 1;
                q.push({res * 10 + dir, tmp.second + 1});
            }

            //光标左移
            // if (!vis[ma][dir - 1])
            // {
            //     vis[ma][dir - 1] = 1;
            //     q.push({ma * 10 + dir - 1, tmp.second + 1});
            // }
        }
        //与第5位数字交换
        if (dir != 5)
        {

            // 5位数字
            l = ma / pos[5] % 10;
            //获取当前位
            r = ma / pos[dir] % 10;
            res = ma - l * pos[5] + r * pos[5] - r * pos[dir] + l * pos[dir];
            if (!vis[res][dir])
            {
                vis[res][dir] = 1;
                if(res==ed)return tmp.second+1;
                q.push({res * 10 + dir, tmp.second + 1});
            }
            //光标右移
            if (!vis[ma][dir + 1])
            {
                vis[ma][dir + 1] = 1;
                q.push({ma * 10 + dir + 1, tmp.second + 1});
            }
        }
        //将当前光标位置的数字+1，上限9
        if (ma / pos[dir] % 10 != 9)
        {
            //获取当前位置数字
            int l = ma / pos[dir] % 10;
            res = ma - l * pos[dir];
            l++;
            res += l * pos[dir];
            if (!vis[res][dir])
            {
                vis[res][dir] = 1;
                if(res==ed)return tmp.second+1;
                q.push({res * 10 + dir, tmp.second + 1});
            }
        }
        //将当前光标位置的数字-1，下限为0
        if (ma / pos[dir] % 10 != 0)
        {
            //获取当前位置数字
            int l = ma / pos[dir] % 10;
            res = ma - l * pos[dir];
            l--;
            res += l * pos[dir];
            if (!vis[res][dir])
            {
                vis[res][dir] = 1;
                if(res==ed)return tmp.second+1;
                q.push({res * 10 + dir, tmp.second + 1});
            }
        }
    }
    return 0;
}
int main()
{
    io;
    string a, b;
    cin >> a >> b;
    st = ed = add;
    int p = 1;
    for (int i = 5; i >= 0; i--)
    {
        st = st + (a[i] - '0') * p;
        ed = ed + (b[i] - '0') * p;
        p *= 10;
    }
    cout << bfs() << "\n";

    // system("pause");
    return 0;
}
/*
051423 980980
26
*/
```