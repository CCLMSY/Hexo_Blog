---
title: 题解|2023暑期牛客多校01
date: 2023/7/17
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_NCD01
description: 题解|2023暑期牛客多校01
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

# D.Chocolate

**博弈题**

## 题意
有一块大小为n*m的巧克力，起始坐标为(1,1)

每次操作可以选择一个坐标(a,b)，并吃掉(1,1):(a,b)中的所有巧克力

每次操作必须吃掉至少一块巧克力，吃掉最后一块巧克力的玩家输

## 解题思路
**1\*1**：显然先行必输

**其他**：对于总数m\*n。若总数为奇数，则先行开局吃偶数个，若总数为偶数，则先行开局吃奇数个。
然后剩余个数变为奇数个，后续每一步，先行者都保证吃完后剩余奇数个巧克力且行数/列数多于1,或1\*1（可以证明在任何状态下，一定有一种这样的操作）。

先行必胜

## 参考程序
```cpp
int solve()
{
    ll n,m;
    cin >> n >> m;
    if(n==1&&m==1) cout << "Walk Alone" << endl;
    else cout << "Kelin" << endl;
    return 0;
}
```

***

# H.Matches

**二分、包络**

## 题意
给定两个长度为 $n$ 的序列 $a,b$ ，定义他们之间的距离为对应元素差的绝对值之和，即：
$$
\sum\limits_{i=1}^n |a_i-b_i|
$$
可以对其中一个序列交换两个数，问至多操作一次后的最小距离。

## 解题思路
绝对值的本质就是数轴上区间的大小，把每一对元素看作一个区间，就能将问题转化。

对于某一个区间， $a_i,b_i$ 决定它自身的正反序

对于某两个区间：
覆盖区域有 相交、包络、不交 三种相对关系
正反序有 正序、反序 两种相对关系
组合之下便有6种相对关系

针对两个区间的相对关系的转换，展开以下分类讨论
1.正序相交 $\rightleftarrows$ 正序包络![Img](/images/ACM/2023Summer_NCD01_H_1.png)
2.正序不交 $\rightleftarrows$ 反序包络![Img](/images/ACM/2023Summer_NCD01_H_2.png)
3.反序不交 $\rightleftarrows$ 反序相交![Img](/images/ACM/2023Summer_NCD01_H_3.png)

可以看到绝对值减小的情况只有两种：
1.反序相交 $\Rightarrow$ 反序不交
2.反序包络 $\Rightarrow$ 正序不交
它们对距离的减少量都是重叠区间大小的两倍

综上所述，只需要找出反序相交和反序包络的情况中，重叠区间最大的情况，即可得到答案。

对于输入数据，可以先按照 $a_i,b_i$ 的相对大小，将正序区间和反序区间分别存入两个容器，记为 $S,T$ ，并算出初始值 $sum$ 
对 $S$ 进行预处理，按照区间起点排序，去除被正序包络的区间（不难证明，被正序包络的区间对答案的贡献一定不大于正序包络它的区间）
对于 $T$ 中的每个区间，二分查找 $S$ 中与它有交集的区间，求出重叠区间大小的最大值 $dif$ 

最终答案为 $sum-2\times dif$ 

## 参考程序
```cpp
int solve()
{
    ll n,sum=0,dif=0;
    cin >> n;
    vector<pll> v(n);
    FORLL(i,0,n-1) cin >> v[i].first;
    FORLL(i,0,n-1) cin >> v[i].second;
    vector<pll> S,T;
    vector<ll> Sx,Sy,len;
    for(auto [a,b]:v)
    {
        sum+=abs(a-b);

        if(a<b) S.emplace_back(a,b);
        if(a>b) T.emplace_back(b,a);
    }

    SORT(S);
    ll r=-INF;
    for(auto p:S)
    {
        if(p.second<=r) continue;
        r=p.second;
        Sx.emplace_back(p.first);
        Sy.emplace_back(p.second);
        len.emplace_back(p.second-p.first);
    }//消除S中被正向包络的区间。算法决定Sy递增。

    auto finddif=[&](ll x,ll y){//对于给定的区间
        ll m=Sx.size(),re=0;
        ll l=upper_bound(ALL(Sx),x)-Sx.begin();
        ll r=lower_bound(ALL(Sy),y)-Sy.begin();
        //查找该区间在S中的位置
        if(l>0) re=max(re,min(Sy[l-1],y)-x);//反序相交1
        if(r<m) re=max(re,y-max(Sx[r],x));//反序相交2
        if(r>l) re=max(re,*max_element(len.begin()+l,len.begin()+r));//反序包络
        return re;
    };

    for(auto [x,y]:T) dif=max(dif,finddif(x,y));
    cout << sum-dif*2 << endl;
    return 0;
}

```

***

# J.Roulette

**数学题**

## 题意
初始有 $n$ 元钱，目标为赢得 $m$ 元钱（即共 $n+m$ 元钱）
每次投注 $x$ 元，有50%的概率输掉，50%的概率赢得 $2x$ 元
第一局投注 $1$ 元，接下来的每局按下述策略投注：
$$\begin{cases}
    x_i=1 , \text{ 上一局赢了} \newline
    x_i=2 \times x_{i−1} ， \text{ 上一局输了}
\end{cases}$$
如果某局剩下的钱不足以按上述策略投注，则失败退场
问达成目标的概率有多大

## 解题思路
投注采用的是**倍投法**，考虑每一轮投注为：输若干局直到赢一局
1. 第一局就赢了，则获得 $1$ 元
2. 第 $n$ 局赢了，则获得 $2^{n-1} - (2^{n-2}+2^{n-3}+\cdots+2+1)=1$ 元 

综上，**每一轮赢取的钱数固定为 $1$ 元**

根据题意，直到输到无法投注才失败，则对于每轮投注
1. 失败的概率为： $\dfrac{1}{2^a}$ 
2. 成功的概率为： $1-\dfrac{1}{2^a}$

其中 $a$ 是这轮投注的局数
那么对于整个投注过程，成功的概率为：$\prod\limits_{i=n}^{n+m-1} (1-\dfrac{1}{2^{a_i}})$


考虑本金与可以进行的投注轮数的关系，进行第a轮至少需要的本金为：
$1+2+\cdots+2^{a-1}=2^a-1$
以此为边界对集合 $\{n,n+1,\cdots,m+n-1\}$ 做划分（划分内投注轮数相同，成功率相同），分别计算每个划分的成功率，再相乘得到结果

### 时间复杂度
划分数 $\log n$，快速幂等操作 $\log n$，总复杂度 $O(\log^2n)$

## 参考程序
```cpp
const int MOD=998244353;
#define Get_Mod(x) ((x)%MOD)
ll QuickPow(ll x,ll y){
    ll ans=1;
    while(y){
        if(y&1) ans=Get_Mod(x*ans);
        x=Get_Mod(x*x);
        y>>=1;
    }
    return ans;
}
ll Get_Inv(ll a){
    return QuickPow(a,MOD-2);
}
int solve()
{
    ll n,m;
    cin >> n >> m;
    ll a,b,c;
    ll base=0,t=(n+1)>>1;
    //base存储当前划分的投注轮数
    while(t){
        base++;
        t>>=1;
    }
    ll l=n,r=pow(2,base+1)-2;
    //l,r分别记录当前划分的左右边界
    ll re=1;
    if(m+n<=r)//所有操作数都在一个划分中，则m+n-1作为右边界
    {
        a=QuickPow(2,base);
        b=Get_Mod(Get_Mod(a+MOD-1)*Get_Inv(a));
        //b为等比数列求和
        c=QuickPow(b,m);//这个划分共m局游戏
        re=Get_Mod(re*c);
        base++;
    }
    else{
        while(r<=m+n-1)
        {
            a=QuickPow(2,base);
            b=Get_Mod(Get_Mod(a+MOD-1)*Get_Inv(a));
            c=QuickPow(b,r-l+1);//完全的划分共r-l+1局游戏
            re=Get_Mod(re*c);

            base++;
            l=pow(2,base)-1;
            r=pow(2,base+1)-2;
        }
        if(l<m+n)
        {
            a=QuickPow(2,base);
            b=Get_Mod(Get_Mod(a+MOD-1)*Get_Inv(a));
            c=QuickPow(b,m+n-l);//最后一个的划分共m+n-l局游戏
            re=Get_Mod(re*c);
        }
    }
    cout << re << endl;
    return 0;
}
```

***

# K.Subdivision

**图论-BFS最短路**

## 题意
给定一个无权无向图 $G=(V,E),|V|=n,|E|=m$ 

每次操作可以选定一条边，在其中插入一个点使其分裂成两条边

问操作任意次数后，与点 $v_1$ 距离不大于 $k$ 的点至多有多少个

## 解题思路
对于无向图 $G$ ，以点 $v_1$ 为根，用BFS确定最短路，刻画BFS树

可以很容易得到以下结论：
1.不在BFS树上的边可以无限分裂，对BFS树上点的距离无影响
2.分裂操作对答案有贡献的情况是：这条边至少有一个端点的距离 $dist_i$ 小于 $k$ 。从端点的角度，分裂这条边的最大贡献是 $k-dist_i$ 

那么可以对BFS树上的点进行分类讨论：
1.在图上具有除父节点外的其他邻节点：分裂以它为端点的树边，或导致其它邻节点到根的距离增大，对答案有损失。故只允许它在非树上的有效边上分裂
2.在图上没有除父节点外的其他邻节点：允许其在回溯的树边上分裂，对答案没有损失，只有贡献

我赛时写的程序和标程其实差不多，只是后面想歪了写了个图存储BFS树然后卡死在那了//
所以本篇参考程序后半部分~~照抄~~借鉴了标程，并加入了对BFS的优化：
由于距离 $dist_i$ 大于 $k$ 的点 $v_i$ 上一定没有对答案有贡献的边（这是最短路的特性决定的，不难证明），BFS只需搜索完 $dist_i$ 小于等于 $k$ 的点并加入BFS树即可，在 $k<<n$ 的情况下可以大幅减少BFS时间

## 参考程序
```cpp
int solve()
{
    ll n,m,k;
    cin >> n >> m >> k;
    vector<vector<ll>> G(n+1);
    ll u,v; pll tpll;
    vector<ll>::iterator it;
    FORLL(i,1,m)
    {
        cin >> u >> v;
        G[u].emplace_back(v);
        G[v].emplace_back(u);
    }
    queue<ll> q;
    vector<ll> dist(n+1,-1);//记录最短路长度
    vector<ll> pre(n+1,0);//记录BFS树上点的前驱
    vector<ll> leaf(n + 1,0);//0表示叶子节点（无子节点）
    q.push(1);dist[1]=0;
    
    //BFS最短路
    while(!q.empty()&&dist[q.front()]<k)//优化
    {
        u=q.front();
        q.pop();
        for (auto v: G[u]){
            if (dist[v]!=-1) continue;
            q.push(v);
            dist[v]=dist[u]+1;
            pre[v]=u;
            leaf[u]=1;
        }
    }

    ll re=1;
    FORLL(i,2,n){
        if(dist[i]==-1||dist[i]>k) continue;
        ll cnt=0;
        for(auto p:G[i]){
            if(p==pre[i]||i==pre[p]) continue;
            ++cnt;
        }//对于非叶子节点，在非树上的有效边上分裂
        if (!leaf[i]) cnt = max(cnt, 1ll);
        //对于叶子节点，如果没有其他邻节点(cnt==0)，允许其在回溯的树边上分裂
        re+=(k-dist[i])*cnt+1;
    }

    cout << re << endl;
    return 0;
}
```