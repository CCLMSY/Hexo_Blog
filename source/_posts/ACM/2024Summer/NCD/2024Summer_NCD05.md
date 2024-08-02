---
title: 题解|2024暑期牛客多校05
date: 2024/7/30
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD05
description: 题解|2024暑期牛客多校05
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

比赛链接：[2024牛客暑期多校训练营5](https://ac.nowcoder.com/acm/contest/81600)

> 玲珑骰子安红豆，入骨相思知不知。  ——温庭筠

# B.珑
## 题意
使用若干个 $1\times 2$ 的小矩形，恰好覆盖一个 $n\times m$ 的大矩形（即不允许重叠、不允许有部分超出大矩形范围）

对于任意两个小矩形，可能存在以下两种限制（中的0种、1种或2种）：
1. 边长为1的边不能相贴
2. 边长为2的边不能相贴，即使相贴部分的长度仅为1

问是否存在一种方案，在满足给定限制的条件下，恰好覆盖大矩形。

## 解题思路
不妨令 $n<m$ ：

1. 小矩形的面积是2，所以 $n\times m$ 必须是偶数
2. 只有 $n=1$ 的情况满足条件2
3. 只有 $n=1且m>2$ 的情况不满足条件1

![image](/images/ACM/2024Summer_NCD05_B.png)

## 参考代码
```cpp
void solve()
{
    ll n,m,a,b;
    cin >> n >> m >> a >> b;
    if(n>m) swap(n,m);
    if(n%2&&m%2) {NO;return ;} // 面积是奇数
    if(n==1&&m==2) {YES;return ;} // 1*2只需要一块
    if(b==0&&n!=1) {NO;return ;} // 只有1*m时能满足b
    if(a==0&&n==1) {NO;return ;} // 只有1*m时不能满足a
    YES;
}
```

# E.安
## 题意
May和Ray各有 $n$ 个骑士，分别按顺序排成一排，编号为 $1\sim n$ 。
May的骑士的生命值为 $a_i$ ，Ray的骑士的生命值为 $b_i$ 。

玩家每次操作可以选择一个双方骑士都存活的序号 $i$ ，命自己的骑士 $i$ 攻击对方的骑士 $i$ （使对方的骑士 $i$ 的生命值减少 $1$ ）。

May先行，两人都执行最优策略，直到无法操作。

问May最终剩余多少个骑士。

## 解题思路
考虑最优策略：
1. $a_i>b_i$ ：优势，在这个位置只需要在对方攻击后反击，就能保证消灭对方。
2. $a_i<b_i$ ：根据上一条，这个位置的骑士无法存活。
3. $a_i=b_i$ ：先攻击的棋子存活，一半的棋子存活，先手可以向上取整。

按照最优策略，答案为 $cnt(a_i>b_i)+\lceil cnt(a_i=b_i)/2 \rceil$ 。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    create_vec(a,n);
    create_vec(b,n);
    ll ans=0,cnt=0;
    FORLL(i,0,n-1){
        if(a[i]==b[i]) cnt++;
        if(a[i]>b[i]) ans++;
    }cout << ans+(cnt+1)/2 << endl;
}
```

# H.入
## 题意
给定一个 $n$ 个点 $m$ 条边的无向图，每个点带唯一点权 $w_i$ 。

棋子初始被放在一个点上，每次会移动到相邻的点中点权最小的一个点。

现在点权和初始位置可以自己决定，问最多能经过多少个点。

## 解题思路
若从点 $i$ 走到点 $j$ ，其他和 $i$ 相邻的点的权值一定大于点 $j$ 的权值，因此之后都不会再走到这些点。

换句话说，走到下一个点之后，就可以把上一步的点和它的相邻点删掉了。

建图，按照以上思路，从每个点出发各一次，DFS找到最长路径长度。

## 参考代码
```cpp
ll n,m;
vector<vector<ll>> G;
vector<int> vis;
ll ans=0,cnt=0;
void dfs(ll u,ll cur=1){
    chmax(ans,cur);
    vector<ll> tmp;
    for(auto v:G[u]) if(!vis[v]){
        tmp.emplace_back(v);
        vis[v]=1;
    }
    cnt+=tmp.size();
    if(n-cnt+cur>=ans)
        for(auto v:tmp) dfs(v,cur+1);
    for(auto v:tmp) vis[v]=0;
    cnt-=tmp.size();
}
void solve()
{
    cin >> n >> m;
    ll u,v;
    G.clear(); G.resize(n+1);
    vis.clear(); vis.resize(n+1,0);
    ans=0;
    FORLL(i,1,m){
        cin >> u >> v;
        G[u].emplace_back(v);
        G[v].emplace_back(u);
    }
    for(ll i=1;i<=n;i++) {
        vis[i]=1; cnt=1;
        dfs(i);
        vis[i]=0;
    }
    cout << ans << endl;
}
```

# L.知
## 题意
给定一个长度为 $n$ 的序列 $a$ ，每次操作可以选择一个下标 $i<n$ ，执行：$a_i=a_i+1,a_{i+1}=a_{i+1}-1$ 。

求任意次操作后，序列 $a$ 的积的最大值 $mod\ 998244353$ 。

## 解题思路
小学老师教过我们，当和一定时，数越平均，积越大。

观察到操作的特性：后面的大数字可以匀到前面，但是前面的数字不能匀给后面。

从前往后处理，把第 $i$ 个数 $a_i$ 加入答案时，
从当前的 前 $i-1$ 个数中，从小到大依次选取尽可能多的数（记为 $b_1\sim b_k$ ），
满足：$max\{b_1\cdots b_k\}< avg\{b_1 \cdots b_k,a_i\}$

然后可以将 $\{b_1 \cdots b_k,a_i\}$ 平均化，是最优的平均方法。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    // ll n=5;
    vector<ll> v(n),vans;
    for(auto &x:v) cin >> x;
    vans.emplace_back(v[0]);
    FORLL(i,1,n-1){
        if(v[i]>vans[0]){
            ll tsum=v[i],j=0;
            while(j<i){ //取出需要被匀的数
                if(vans[j]>=tsum/(j+1)) break;  
                //如果当前的数已经大于等于平均值，就不需要被平均了
                tsum+=vans[j]; j++;
            }
            vector<ll> temp;
            FORLL(k,j,i-1) temp.emplace_back(vans[k]);
            ll avg=tsum/(j+1),tt=tsum%(j+1);
            temp.insert(temp.end(),tt,avg+1);
            temp.insert(temp.end(),j+1-tt,avg);
            vans=temp;
        }else vans.emplace_back(v[i]);
        SORT(vans);
    } // print_vec(ans);
    ll ans=1;
    for(auto x:vans) multo(ans,x);
    cout << ans << endl;
}
```