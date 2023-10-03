---
title: 题解|J.Fine Logic-2023暑期牛客多校03
date: 2023/7/24
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 思维
  - 图论
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 63d1969b
description: 题解|J.Fine Logic-2023暑期牛客多校03 Idx:4 思维·拓扑排序
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

# J.Fine Logic
**拓扑排序**
## 题目大意
对于 $n$ 个人（记为 $1-n$），给定 $m$ 对偏序关系，表示胜利关系
对于一组排名，位于前面的人视为赢过后面的人
问如何构造最少组数的排名，使得满足给定的 $m$ 对胜利关系

## 解题思路
对于 $n$ 个人之间的所有胜利关系，构造两组排名：
$$
1,2,3,\cdots,n \newline
n,n-1,n-2,\cdots,1
$$
一定能满足所有胜利关系

如果 $m$ 个胜利关系构成DAG，则将它的拓扑排序列作为排名输出一组即可
否则输出上面两组排名

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll n,m;
    cin >> n >> m;
    vector<vector<ll>> G(n+1),pre(n+1);
    vector<pll> E;
    vector<ll>::iterator it;
    ll u,v;
    FORLL(i,1,m){
        cin >> u >> v;
        E.emplace_back(u,v);
    }
    SORT(E);
    unique(ALL(E));
    for(auto e:E){
        G[e.first].emplace_back(e.second);
        pre[e.second].emplace_back(e.first);
    }
    FORLL(i,1,n){
        SORT(G[i]);
        SORT(pre[i]);
    }
    ll cnt=0;
    int flag=1;
    vector<ll> vec;
    bitset<1000005> f;
    f.reset();
    while(flag){
        flag=0;
        FORLL(i,1,n) if(f[i]==0){
            while(pre[i].size()&&f[pre[i].back()]) pre[i].pop_back();
            if(f[i]==0&&pre[i].empty()){
                vec.emplace_back(i);
                f[i]=1;flag=1;cnt++;
            }
        }
    }
    if(cnt==n){
        cout << "1" << endl;
        for(auto i:vec) cout << i << ' ';
        cout << endl;
        return;
    }else{
        cout << "2" << endl;
        FORLL(i,1,n) cout << i << ' ';
        cout << endl;
        for(ll i=n;i>0;i--) cout << i << ' ';
        cout << endl;
    }
    
}
```