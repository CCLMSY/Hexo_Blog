---
title: 题解|K.Subdivision-2023暑期牛客多校01
date: 2023/7/17
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 图论
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 3911230d
description: 题解|K.Subdivision-2023暑期牛客多校01 Idx:3 图论
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

# K.Subdivision

**图论-BFS最短路**

## 题目大意
给定一个无权无向图 $G=(V,E),|V|=n,|E|=m$ 

每次操作可以选定一条边，在其中插入一个点使其分裂成两条边

问操作任意次数后，与点 $v_1$ 距离不大于 $k$ 的点至多有多少个

## 解题思路
对于无向图 $G$ ，以点 $v_1$ 为根，用BFS确定最短路，刻画BFS树

可以很容易得到以下结论：
1.不在BFS树上的边可以无限分裂，对BFS树上点的距离无影响
2.分裂操作对答案有贡献的情况是：这条边至少有一个端点的距离 $dist_i$ 小于 $k$ 。从端点的角度，分裂这条边的最大贡献是 $k-dist_i$ 

那么可以对BFS树上的点进行分类讨论：
1.在图上具有除父结点外的其他邻结点：分裂以它为端点的树边，或导致其它邻结点到根的距离增大，对答案有损失。故只允许它在非树上的有效边上分裂
2.在图上没有除父结点外的其他邻结点：允许其在回溯的树边上分裂，对答案没有损失，只有贡献

我赛时写的程序和标程其实差不多，只是后面想歪了写了个图存储BFS树然后卡死在那了//
所以本篇参考程序后半部分~~照抄~~借鉴了标程，并加入了对BFS的优化：
由于距离 $dist_i$ 大于 $k$ 的点 $v_i$ 上一定没有对答案有贡献的边（这是最短路的特性决定的，不难证明），BFS只需搜索完 $dist_i$ 小于等于 $k$ 的点并加入BFS树即可，在 $k<<n$ 的情况下可以大幅减少BFS时间

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

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
    vector<ll> leaf(n + 1,0);//0表示叶子节点（无子结点）
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
        //对于叶子节点，如果没有其他邻结点(cnt==0)，允许其在回溯的树边上分裂
        re+=(k-dist[i])*cnt+1;
    }

    cout << re << endl;
    return 0;
}
```