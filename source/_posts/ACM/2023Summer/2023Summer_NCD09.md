---
title: 题解|2023暑期牛客多校09
date: 2023/8/14
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_NCD09
description: 题解|2023暑期牛客多校09
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

# E.Puzzle: Square Jam
**几何**
## 题目大意
给定一个 $n$ 行 $m$ 列的矩形，将其切割为若干个边长为正整数的正方形部分
要求矩形内每个整点都不能同时在四个正方形的边界上

## 解题思路
每次在矩形中取最大的正方形，剩余部分递归处理即可。如此取，每个点最多在三个正方形的边界上

## 参考代码
```cpp
vector<pair<pll,ll>> ans;
pair<pll,ll> tp;//<坐标,边长>
void solve_sqr(ll x,ll y,ll n,ll m){
    //处理坐标在(x,y)，n行m列的矩形
    if(n==0||m==0) return ;
    if(n>=m){
        tp.first.first=x;tp.first.second=y;tp.second=m;
        ans.push_back(tp);
        solve_sqr(x+m,y,n-m,m);
    }else{
        tp.first.first=x;tp.first.second=y;tp.second=n;
        ans.push_back(tp);
        solve_sqr(x,y+n,n,m-n);
    }
}
void solve()
{
    ll m,n;cin >> n >> m;
    ans.clear();
    solve_sqr(0,0,n,m);
    cout << YES ;
    cout << ans.size() << endl;
    for(auto x:ans){
        cout << x.first.first << ' ' << x.first.second << ' ' << x.second << endl;
    }
}
```