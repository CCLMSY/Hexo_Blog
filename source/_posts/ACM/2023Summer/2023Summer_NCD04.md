---
title: 题解|2023暑期牛客多校04
date: 2023/7/28
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 6e0e8bd8
description: 题解|2023暑期牛客多校04
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
highlight_shrink:
aside:
---

# A.Bobo String Construction
**构造**
## 题目大意
给定一个标识01串 $t$ ，构造一个长度为 $n$ 的信息01串 $s$ ，使得串 $m=t+s+t$ 中，串$t$ 仅在开头和结尾各出现过一次。（其中 $+$ 表示字符串连接运算）
## 解题思路
考虑构造 $s$ 全为 $0$ 或 $1$ 

对于特殊情况， $t$ 全为 $0$ 或 $1$ ，则使 $s$ 全为 $1$ 或 $0$ 即可

如果 $t$ 中兼有 $0$ 和 $1$ ，则 显然 $s$ 中不可能有子串 $t$
但对于 $m=t+s+t$ ，可能有 $t的后缀+s+t的前缀=t$ 的情况出现，导致不满足题设条件//

那么下面感性证明一下两种方案至少有一种成立。
假设对于兼有 $0$ 和 $1$ 的 $t$ ，在 $s$ 全为 $0$ 的情况下出现：
$t的后缀+s+t的前缀=t$ 
那么构造 $s$ 全为 $1$ 即可，反之亦然

于是问题就转化为在 $s$ 全为 $1$ 或 $0$ 的方案中选择一种使得串$t$ 仅在开头和结尾各出现过一次
先使 $s$ 全为 $1$ 将 $m=t+s+t$ 去掉首尾一个字符，判断中间有无字串 $t$ 即可
归纳发现特殊情况也可以用这种方法构造

### 时间复杂度
数据范围小（ $n\le 1000,|t|\le 1000$ ），BF即可

## 参考程序
```cpp
void solve()
{
    string t;ll n;
    cin >> n >> t;
    string s;FORLL(i,1,n) s.push_back('1');
    string m=t+s+t;m.erase(m.begin());m.pop_back();
    if(m.find(t)!=string::npos) FORLL(i,1,n) cout << '0';
    else FORLL(i,1,n) cout << '1';
    cout << endl;
}
```

***

# F.Election of the King
**签到、模拟**
## 题目大意
$n$ 个人竞选国王，每个人的从政理念用 $a_i$ 量化表示
每人每轮可以进行投票，对于个人而言，他会将票投给和他自己的政治理念相差最大的人
票数最多的人将被淘汰。如果票数相同，则淘汰值最大的；如果最大值有多个，则淘汰序号最大的人。
重复直到剩余一人成为国王，问最后谁会竞选上国王
## 解题思路
对于其中某一个人，和他相差最大的，一定是最大值与最小值之一
那么每次一定淘汰最大值或最小值
因此对所有人进行排序，每轮投票处于中间位置的人代表着多数人的意志
因此每轮看中间的人投最大值还是最小值，踢到只剩一人即可

## 参考程序
```cpp
void solve()
{
    ll n,t;
    cin >> n;
    deque<pll> v;
    FORLL(i,1,n){
        cin >> t;
        v.emplace_back(t,i);
    }SORT(v);
    float mid;
    while(n>1){
        mid=1.*(v.front().first+v.back().first)/2;
        if(v[(n+1)/2-1].first<=mid) v.pop_back();
        else v.pop_front();
        n--;
    }cout << v.front().second << endl;
}
```

***

# H.Merge the squares!
**几何、分治**
## 题目大意
给定一个 $n\times n$ 的矩阵，它由 $n\times n$ 个小正方形组成
每次操作可以选择 $2\le x\le50$ 个正方形并把它们组合成一个更大的正方形（组合后的形状也必须为正方形）
求合并到整个边长为 $n$ 大小的正方形的操作过程
## 解题思路
这道题可以采用分治的思想

对于边长不大于 $7$ 的正方形，它的面积不大于 $49$ ，可以直接合并
对于边长较大的正方形，可以考虑将其分解为更小的正方形，递归处理

接下来考虑一种通用可行的分解方法如下图
![Img](/images/ACM/2023Summer_NCD04_H.png)
这种分法将大正方形分解成两个小正方形和两个矩形
假设小正方形在递归处理中合并完成，占用块数为 $2$ ，那么每个长方形部分允许占用的块数为 $(50-2)/2=24$ 块

对于每个长方形，按照宽分割成一个正方形和一个矩形，对两部分分别递归处理

代码中预处理了一个切割方案，用于决定在大正方形边长为 $i$ 的情况下，满足矩形 $(i-j)\times j$ 即长方形的部分，占用分割块数不超过 $24$ 时，左上角的正方形边长，以此保证每次递归处理大正方形都满足条件

# 参考代码
```cpp
ll len[1005]={0};
void pre_len(ll n){
    FORLL(i,2,n)
        FORLL(j,1,i-1){
            ll cnt=0;
            ll a=j,b=i-j;//先减去一个j*j大小的方形
            while(b){
                cnt+=a/b;
                a%=b;
                swap(a,b);
            }//辗转相减得到(i-j)*j下分块数量
            if(cnt <= 24){
                len[i]=j;
                break;
            }//得到长i下的可用宽j
        }
}
struct node_op{
    ll x,y,n;
}top;
vector<node_op> op;
void dfs(ll x1,ll y1,ll x2,ll y2)//递归处理(x1,y1):(x2,y2)区域的矩形
{
    ll difx=x2-x1+1,dify=y2-y1+1;
    if(difx==1||dify==1) return;
    if(difx>dify){
        dfs(x1,y1,x1+dify-1,y2);
        dfs(x1+dify,y1,x2,y2);
    }else if(difx<dify){
        dfs(x1,y1,x2,y1+difx-1);
        dfs(x1,y1+difx,x2,y2);
    }else{//difx==dify
        if(difx==1) return;//边长为1无需merge
        if(difx>7){
            dfs(x1,y1,x1+len[dify]-1,y1+len[difx]-1);
            dfs(x1+len[dify],y1+len[difx],x2,y2);
            dfs(x1+len[dify],y1,x2,y1+len[difx]-1);
            dfs(x1,y1+len[difx],x1+len[dify]-1,y2);
        }
        top.x=x1;top.y=y1;top.n=difx;
        op.emplace_back(top);
    }
}
void solve()
{
    op.clear();
    ll n;
    cin >> n;
    if(n==1) {cout << "0" << endl;return;}
    pre_len(n);
    dfs(1,1,n,n);
    cout << op.size() << endl;
    for(auto i:op){
        cout << i.x << ' ' << i.y << ' ' << i.n << endl;
    }
}
```

***

# J.Qu'est-ce Que C'est?
**动态规划**
## 题目大意
给定两个正整数 $n,m$ ，要求构造长度为 $n$ 的整数序列 $a$ ，满足：
1. $\forall i\in [1,n],-m\le a_i\le m$
2. 任意长度大于 $1$ 的连续子段之和不小于 $0$

求满足以上条件的整数序列的个数，结果取模
## 解题思路
计数取模，比起DP我会首先考虑猜通项
赛时根据样例1：$n=3,m=3\Rightarrow130$ 以及手算的 $n=2,m=3\Rightarrow28$ ，结合题目特征，拟出了一个满足上面两个情况的很奇怪的式子//然后样例2过不了，~~开摆~~

DP的思想是先计算出第 $i$ 个位置前已经满足题设条件，且最小后缀和为 $j$ 的方案数量（ $j$ 可以是负数，因为最后一个数可以单独为负数）
考虑如何进行状态转移

对每个位置从大到小遍历当前位置填数后的最小后缀和 $j$ ，记当前遍历到 $i$ ，填入的数为 $x$
1. 对于非负整数 $j$ ，填入 $x$ 时的方案数为 $dp_{i-1,j-x}$ 
其中 $j-x\in[-m,m],x\in[-m,m],j\ge 0 \Rightarrow x\in [j-m,m]$，此时 $dp_{i,j}=\sum\limits_{x=j-m}^m dp_{i-1,x}$
2. 对于负整数 $j$ ，此时 $x=j$ ，其方案数为允许 $x$ 填入的所有方案数之和，即 $dp_{i,j}=\sum\limits_{x=-j}^m dp_{i-1,x}$

由于每次计算都用到了前一个位置的以 $m$ 为上界的区间和，因此可以对每个位置的dp数组从 $m$ 到 $-m$ 维护一个前缀和，以降低时间复杂度

## 复杂度
时间复杂度： $O(n^2)$
空间复杂度： $M(Cn^2)$ ， $C$ 是常数，开LL可能会导致MLE
7/30：开L都MLE了//进行了空间优化，只保留 $dpsum$ 数组

## 参考代码
```cpp
#define O 5000 //记为0
long dp[5005][10005]={0};
//表示前i-1个数的最小后缀和为j+5000时，满足条件的可行方案数
long dpsum[5005][10005]={0};
//表示dp[i][m]到dp[i][j]可行方案数的区间和
void solve()
{
    ll m,n;
    cin >> n >> m;
    for(ll j=m;j>=-m;j--) {dp[1][j+O]=1;dpsum[1][j+O]=dpsum[1][j+1+O]+dp[1][j+O];}
    //预处理前0位
    FORLL(i,2,n){
        for(ll j=m;j>=-m;j--){
            if(j>=0) dp[i][j+O]=dpsum[i-1][j-m+O];
            //下界x+m=j => x=j-m
            else dp[i][j+O]=dpsum[i-1][-j+O];
            //j<0 下界-j
            dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dp[i][j+O]);
            //计数取模
        }
    }cout << dpsum[n][-m+O] << endl;
}
```
7/30：空间优化后
```cpp
#define O 5000 //记为0
//long dp[5001][10001]={0};//表示前i-1个数最小后缀和为j+5000，满足条件的可行方案数
long dpsum[5001][10001]={0};//表示dp[i][m]到dp[i][j]可行方案数的区间和
void solve()
{
    ll m,n;
    cin >> n >> m;
    for(ll j=m;j>=-m;j--){
        //dp[1][j+O]=1;
        //dpsum[1][j+O]=dpsum[1][j+1+O]+dp[1][j+O];
        dpsum[1][j+O]=dpsum[1][j+1+O]+1;
    }
    FORLL(i,2,n){
        for(ll j=m;j>=-m;j--){
            //if(j>=0) dp[i][j+O]=dpsum[i-1][j-m+O];//下界x+m=j => x=j-m
            if(j>=0) dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dpsum[i-1][j-m+O]);
            //else dp[i][j+O]=dpsum[i-1][-j+O];//j<0 下界-j
            else dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dpsum[i-1][-j+O]);
            //dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dp[i][j+O]);
            
        }
    }cout << dpsum[n][-m+O] << endl;
}
```

***

# L.We are the Lights
**思维题**
## 题目大意
有 $n\times m$ 的电灯矩阵，初始全为关闭状态

每次操作会打开或关闭某一行/列的所有灯

问在给定的 $q$ 次操作后，共有多少盏灯亮着

## 解题思路
这道题和某刷墙小游戏类似（）玩过的话应该不难想到做法
![Img](/images/ACM/2023Summer_NCD04_L.jpg)
后面的行动会覆盖前面的行动，用数组记录当前行/列是否已被覆盖

从后往前遍历操作即可

## 参考代码
```cpp
bool rc[N]={0},op[N]={0},rowvd[N]={0},colvd[N]={0};
ll x[N]={0},cnt=0,cntr=0,cntc=0;
void solve()
{
    ll n,m,q;
    cin >> n >> m >> q;
    string trc,top;ll t;
    FORLL(i,1,q){
        cin >> trc >> t >> top;
        if(trc=="row") rc[i]=1;else rc[i]=0;
        if(top=="on") op[i]=1;else op[i]=0;
        x[i]=t;
    }
    for(ll i=q;i>0;i--){
        if(rc[i]){
            if(rowvd[x[i]]==0){
                rowvd[x[i]]=1;cntr++;
                if(op[i]==1) cnt+=m-cntc;
            }
        }else{
            if(colvd[x[i]]==0){
                colvd[x[i]]=1;cntc++;
                if(op[i]==1) cnt+=n-cntr;
            }
        }
    }cout << cnt << endl;
}
```