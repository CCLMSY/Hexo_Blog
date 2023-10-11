---
title: 题解|A.Tree-2023暑期牛客多校06
date: 2023/8/4
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 动态规划
  - 图论
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: d9c7e69f
description: 题解|A.Tree-2023暑期牛客多校06 Idx:5 动态规划·Kruskal重构树
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

# A.Tree
**图论-Kruskal、动态规划**
## 题目大意
给定一棵 $n$ 个节点带点权和边权的无根树

节点具有颜色白（$0$）和黑（$1$），颜色可反转，所需代价 $cost_i$ 为该点点权

整棵树的 $earning$ 为 $\sum\limits_{u\in V_0}\sum\limits_{v\in V_1} val(u,v)$ 。其中， $val(u,v)$ 为节点 $u\rightarrow v$的最短路径上的最大边权， $V_0$ 为白色点集， $V_2$ 为黑色点集

可以操作反转节点颜色任意次，求最大 $earning-\sum cost$ （以下简称 $score$ ）

## 解题思路
注意到对于每对黑白点对，其贡献为最短路径上的最大边权。考虑利用Kruskal算法对树进行重构，即按边权从小到大的顺序进行加边

可以发现，在加入某条边 $e_i$ 时，原本在同一连通分量中的黑白点对的 $score$ 不受影响。由于加边顺序，新加的边一定具有目前最大的边权，因此只有经过新加的这条边的点对才对 $score$ 具有贡献，每个点对的贡献值为 $w_i$ ，点对数量为：$左白\times右黑+左黑\times右白$

构造dp数组：定义 $dp_{i,j}$ 为连通分量 $i$ （以并查集中连通分量的根节点标识）中具有 $j$ 个白色节点时的最大 $score$ 。初始对于点 $i$ ，$dp_{i,color_i}=0$ （不变）；$dp_{i,color_i\oplus 1}=-cost_i$ （反转）

在合并 $A、B$ 两个连通分量到 $C$ 时具有以下转移方程：
$$
dp_{C,i}=\max\limits_{0\le k \le |A| \And 0\le i-k\le |B|}\{dp_{A,k}+dp_{B,i-k}+w(k(|B|-(i-k))+(i-k)(|A|-k))\}
$$
（两边原有的 $score$ 加上过新加边的 $score$ ）

可以结合代码注释进行理解


### 时间复杂度
$O(n^2\log n)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
struct edge{
    int u,v;ll w;

    edge():u(0),v(0),w(0){}
    edge(int _u,int _v,ll _w): u(_u),v(_v),w(_w) {}

    bool operator<(const edge &b)const{
        return w<b.w;
    }
};//手写edge类，自定义比较方式
//下面的注释中也添加了不用手写的代码，优点是少写一段代码，缺点是表述较为冗杂不直观
#define N 3005
int dsu[N]={0},sz[N]={0},color[N]={0};
vector<ll> dp[N];//dp_i,j:集合i有j个白色点时的score
vector<edge> Edge;
//vector<pair<ll,pll>> Edge;//<w,<u,v>>;
vector<ll> cost(N);
ll find_dsu(ll x){
    return dsu[x]==x?x:dsu[x]=find_dsu(dsu[x]);
}//并查集
void merge_dsu(ll a,ll b,ll w){
    a=find_dsu(a);b=find_dsu(b);
    if(sz[a]>sz[b]) swap(a,b);
    if(a==b) return ;

    ll n=sz[a]+sz[b],rt;
    //n:a,b点集体积之和，即白点数量上限
    vector<ll> tmp(n+1);
    FORLL(i,0,n){
        rt=-INF;
        for(ll k=0;k<=sz[a];k++) if(k<=i&&i-k<=sz[b]){//k:a中白点数量
            ll cnt0=i-k,cnt1=sz[b]-(i-k);//cnt0:b中白点数量 cnt1:b中黑点数量
            rt=max(rt,dp[a][k]+dp[b][i-k]+w*(k*cnt1+cnt0*(sz[a]-k)));
            //dp=max(两点集已经具有的最大earning-cost+经过这条边的点对数*边权)
        }
        tmp[i]=rt;
    }
    
    dp[b]=tmp;

    sz[b]+=sz[a];
    dsu[a]=b;

}
void solve()
{
    ll n,t;cin >> n;
    Edge.resize(n-1);
    ll u,v,w;
    FORLL(i,1,n){
        dp[i].resize(2);//接下来有需求会扩容
        dsu[i]=i;sz[i]=1;
    }
    FORLL(i,1,n) cin >> color[i];
    FORLL(i,1,n){
        cin >> cost[i];
        dp[i][color[i]]=(ll)0;
        dp[i][color[i]^1]=(ll)-cost[i];
    }
    for(auto &e:Edge){
        cin >> e.u >> e.v >> e.w;
        //cin >> u >> v >> w;
        //e.second.first=u;e.second.second=v;e.first=w;
    }//存边
    SORT(Edge);//按边权从小到大重构树
    edge e;
    //pair<ll,pll> e;
    FORLL(i,0,n-2){
        e=Edge[i];
        u=e.u;v=e.v;w=e.w;
        //u=e.second.first;v=e.second.second;w=e.first;
        merge_dsu(u,v,w);
    }auto tmp=dp[find_dsu(1)];
    cout << *max_element(ALL(tmp)) << endl;
}
```