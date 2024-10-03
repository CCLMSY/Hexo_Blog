---
title: 题解|2024暑期杭电多校07
date: 2024/8/9
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU07
description: 题解|2024暑期杭电多校07
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（7）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A87%A3%A9&source=1&searchmode=source)

# (1010)HDU7514.故障机器人想活下去
## 题意
故障机器人有 $x$ 点血量。
他将按顺序进行 $n$ 场战斗，第 $i$ 场战斗会受到 $a_i$ 点伤害。
故障机器人还有 $k$ 个烟雾弹，每个烟雾弹可以让他跳过一场战斗而不受伤害。

问故障机器人最多能活到第几场战斗结束。

## 解题思路
维护当前最高的 $k$ 个伤害，作为被跳过的伤害。
统计受伤量，直到受伤量超过 $x$ 为止。

## 参考代码
```cpp
void solve()
{
    ll n,x,k;cin >> n >> x >> k;
    create_vec(v,n);
    priority_queue<ll,vector<ll>,less<ll>> pq;
    ll S=0,cnt=0;
    FORLL(i,0,n-1){
        S+=v[i];
        pq.push(v[i]);
        while(S>=x){
            if(cnt>=k){
                cout << pq.size()+cnt-1 << endl;
                return ;
            }
            S-=pq.top();
            pq.pop();
            cnt++;
        }
    }cout << n << endl;
}
```

# (1011)HDU7515.蛋糕上的草莓是蛋糕的灵魂
## 题意
有 $x$ 个相同的草莓和 $y$ 个相同的蛋糕。
每次可以将当前所有草莓或蛋糕放在一起切 $m$ 刀，即份数变为此前的 $2m$ 倍。
在满足条件的情况下切草莓的次数尽可能少。
求出最终草莓和蛋糕的数量。

## 解题思路
贪心的，观察到蛋糕是不用切的，将草莓切到 $\gcd(x,y)$ 份即可。
若 $\gcd$ 是 $x$ 的奇数倍，需要多来一刀。

## 参考代码
```cpp
void solve()
{
    ll x,y;cin >> x >> y;
    ll g=__gcd(x,y);
    if(y/g>1&&(y/g)%2) cout << y << ' ' << x/g*2 << endl;
    else cout << y << ' ' << x/g << endl;
}
```