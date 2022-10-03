#### Question
Hosssam decided to sneak into Hemose's room while he is sleeping and change his laptop's password. He already knows the password, which is a string s of length n. He also knows that there are k special letters of the alphabet: c1,c2,…,ck.

Hosssam made a program that can do the following.

The program considers the current password s of some length m.
Then it finds all positions i (1≤i<m) such that si+1 is one of the k special letters.
Then it deletes all of those positions from the password s even if si is a special character. If there are no positions to delete, then the program displays an error message which has a very loud sound.
For example, suppose the string s is "abcdef" and the special characters are 'b' and 'd'. If he runs the program once, the positions 1 and 3 will be deleted as they come before special characters, so the password becomes "bdef". If he runs the program again, it deletes position 1, and the password becomes "def". If he is wise, he won't run it a third time.

Hosssam wants to know how many times he can run the program on Hemose's laptop without waking him up from the sound of the error message. Can you help him?

#### Input
The first line contains a single integer t (1≤t≤10^5) — the number of test cases. Then t test cases follow.

The first line of each test case contains a single integer n (2≤n≤10^5) — the initial length of the password.

The next line contains a string s consisting of n lowercase English letters — the initial password.

The next line contains an integer k (1≤k≤26), followed by k distinct lowercase letters c1,c2,…,ck — the special letters.

It is guaranteed that the sum of n over all test cases does not exceed 2⋅10^5.

#### Output
For each test case, print the maximum number of times Hosssam can run the program without displaying the error message, on a new line.

#### input
```in
10
9
iloveslim
1 s
7
joobeel
2 o e
7
basiozi
2 s i
6
khater
1 r
7
abobeih
6 a b e h i o
5
zondl
5 a b c e f
6
shoman
2 a h
7
shetwey
2 h y
5
samez
1 m
6
mouraz
1 m
```
#### output 
```out
5
2
3
5
1
0
3
5
2
0
```
#### Note
In the first test case, the program can run 5 times as follows: iloveslim→ilovslim→iloslim→ilslim→islim→slim
In the second test case, the program can run 2 times as follows: joobeel→oel→el
In the third test case, the program can run 3 times as follows: basiozi→bioi→ii→i.

In the fourth test case, the program can run 5 times as follows: khater→khatr→khar→khr→kr→r
In the fifth test case, the program can run only once as follows: abobeih→h
In the sixth test case, the program cannot run as none of the characters in the password is a special character.
#### 思路：
我们可以将整个字符串分为2类，一类是普通字符，一类是特殊字符，因为每一轮删除的字符都是特殊字符前的字符（当前无字符时则不操作删除操作），因此我们只需要统计每个特殊字符间的距离，并取得最大值，就是我们最后所要的结果。

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
#define x first
#define y second
using namespace std;
const int INF = 0x3f3f3f3f;
const int N = 5e5 + 5;
const int M = 1e6;
const int mod = 1e9 + 7;
const int add = 1e6;

bool vis[33];
int main()
{
    io;
    int t,n,k;
    char c;
    string str;
    cin>>t;
    while(t--)
    {
         memset(vis,0,sizeof(vis));
         cin>>n>>str;
         cin>>k;
        while(k--)
        {
            cin>>c;
            vis[c-'a']=1;
        }

        int ans=0;
        int last=0;
        for(int i=0;i<n;i++)
        {
            c=str[i];
            if(vis[c-'a'])
            {
                ans=max(ans,i-last);
                last=i;
            }
        }
        cout<<ans<<"\n";
    }

    system("pause");
    return 0;
}
/*

*/
```