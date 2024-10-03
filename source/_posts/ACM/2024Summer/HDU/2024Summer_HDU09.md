---
title: 题解|2024暑期杭电多校09
date: 2024/8/16
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU09
description: 题解|2024暑期杭电多校09
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（9）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A89%A3%A9&source=1&searchmode=source)

# (1005)HDU7533.怪物猎人
## 题意
攻击生命值为 $k$ 的怪物，每回合可选择造成 $x$ 或 $y$ 点伤害。
分别回答是否有一种攻击序列，可以恰好在第奇数回合/第偶数回合击杀怪物。

## 解题思路
最边缘的情况即为全用 $x$ 和 全用 $y$ ，分别需要 $\lceil \frac{k}{x}\rceil$ 和 $\lceil \frac{k}{y}\rceil$ 回合。

这两个回合数相等，只有一种可能，否则都可以。

## 参考代码
```cpp
void solve()
{
    ll n,a,b;cin >> n >> a >> b;
    ll ta,tb;
    ta=(n+a-1)/a;
    tb=(n+b-1)/b;
    if(ta==tb){
        if(ta%2) cout << "Yes" << endl << "No" << endl;
        else cout << "No" << endl << "Yes" << endl;
    }else{
        cout << "Yes" << endl << "Yes" << endl;
    }
}
```

# (1007)HDU7535.小猫钓鱼
## 题意
有 $2n$ 张牌，点数 $1\sim n$ 各出现2次。
两人各分到 $n$ 张牌，已知每个人手中牌的点数。
两人轮流放牌到牌堆顶，若牌堆中此前已经有相同点数的牌，则将这两张牌以及中间的所有牌拿回手中。
先打完的人输。

## 解题思路
称手牌中成对的牌为双牌，单独的牌为单牌。

当自己打出单牌时，对方必定能打出一样的单牌收回，且先后手不变，这样是不优的。
因此，贪心的打法是先手打双牌，后手打双牌，先手打上次打的双牌。
这样的结果是：后手失去一个双牌，交换先后手。

观察到两人手牌中的双牌的数量相同，因此有双牌的情况下，后手先失去所有双牌，先手获胜。
否则，后手可以稳定收取先手打出的牌，后手获胜。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    map<ll,int> mp;
    ll t;
    FORLL(i,1,n){
        cin >> t;
        mp[t]++;
    }
    FORLL(i,1,n) cin >> t;
    if(mp.size()<n) cout << "shuishui" << endl;
    else cout << "sha7dow" << endl;
}
```

