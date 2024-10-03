---
title: 题解|2024暑期牛客多校07
date: 2024/8/6
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD07
description: 题解|2024暑期牛客多校07
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

比赛链接：[2024牛客暑期多校训练营7](https://ac.nowcoder.com/acm/contest/81702)

# I.Fight Against the Monster
## 题意
瓶子换水问题。 $m$ 个空瓶可以换 $k$ 瓶水， 求一共要喝 $h$ 瓶水，初始最少需要多少瓶水。

## 解题思路
二分check。

设初始有 $x$ 瓶水，换水次数为 $\dfrac{x-m}{m-k}+1=\dfrac{x-k}{m-k}$。
每次换水产生 $k$ 瓶水，总共 $x+k\cdot \dfrac{x-k}{m-k}$ 瓶水。

## 参考代码
```cpp
int check(ll m,ll k,ll h,ll x){
    ll t=(x-k)/(m-k)*k+x;   
    if(t>=h) return 1;
    return 0;
}
void solve()
{
    ll m,k,h;cin >> m >> k >> h;
    if(h<=m){cout << h << endl;return;} //不用换
    if(m<=k){cout << m << endl;return;} //生生不息
    ll l=0,r=1e9+7;
    while(l<r){
        ll mid=(l+r)/2;
        if(check(m,k,h,mid)) r=mid;
        else l=mid+1;
    }
    FORLL(i,max(0ll,l-10),r+10) if(check(m,k,h,i)) {cout << i << endl;return;}
}
```

# J.Ball
## 题意
给定一根左端点为 $(0,0)$ 垂直于y轴长度为 $l$ 的棍子。
在上面找一个支点使得棍子绕支点旋转可以碰到位于 $(x,y)$ 的小球。

## 解题思路
支点位于两端时扫过的面积可覆盖所有情况。
判断小球到两端的距离。

## 参考代码
```cpp
void solve()
{
    ll l,x,y;
    cin >> l >> x >> y;
    ll dis1,dis2;
    dis1=x*x+y*y;
    dis2=(l-x)*(l-x)+y*y;
    if(dis1>l*l&&dis2>l*l) { cout << "No\n"; return ;}
    else cout << "Yes\n";
    if(dis1<dis2) cout << 0 << endl;
    else cout << l << endl;
}
```