---
title: 题解|F.Link with Chess Game-2023暑期牛客多校02
date: 2023/7/21
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 博弈
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: '2e74151'
description: 题解|F.Link with Chess Game-2023暑期牛客多校02  Idx:4 二分图博弈
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

# F.Link with Chess Game
**二分图博弈**
## 题目大意
在一行长度为 $n$ 的格子上有红绿蓝三种颜色的棋子，他们的坐标构成一个有序三元组 $(r,g,b)$ （坐标可以重复）

每次可以选择一颗棋子向左或向右移动一步，如果某次移动后的三元组 $(r_i,g_i,b_i)$ 在此前出现过，则执行移动的人失败

## 解题思路
### 赛时想法
假设只有一颗棋子的情况
选定一个方向，假设棋子当前距离这个方向端点的距离为 $x$ 。不难证明 $x$ 为奇数时胜利； $x$ 为偶数时失败
在这种情况下，只要当前位置与两端之一的距离为奇数即可获胜
基于这一结论，猜想三颗棋子的条件下，每颗棋子与左、右两端的距离之和 $suml,sumr$ 与必胜态之间的关系，和一颗棋子的情况相似
样例满足这一猜想
```cpp
int solve()
{
    ll n,a,b,c;
    cin >> n >> a >> b >> c;
    ll l,r;
    l=a+b+c-3;
    r=n*3-a-b-c;
    if(l&1||r&1) cout << "Alice" << endl;
    else cout << "Bob" << endl;
    return 0;
}
```
### 赛后补题
对于这类无向的博弈，可以考虑是否为二分图博弈
把三颗棋子看作是三个维度，显然所有状态组成的图是一个正方体，一定是二分图
在 $n$ 为偶数的情况下，最大匹配可以覆盖所有状态，即初始状态一定在某一对匹配上。这种情况下，先手一定落在匹配中的最后一个点，因此是必胜的
在 $n$ 为奇数的情况下，对于起始点在较小一部的状态点，一定在某一最大匹配上。这种情况下，先手一定落在匹配中的最后一个点，因此是必胜的
对于起始点在较大一部中的状态点的情况，一定存在一个最大匹配不包含起始点。因此在这个最大匹配上，后手一定落在匹配中的最后一个点，因此是必败的
赛时的思路虽然不严谨，但误打误撞结论和正解是一致的//

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
int solve()
{
    ll n,a,b,c;
    cin >> n >> a >> b >> c;
    if(n&1)
        if((a+b+c-1)&1) cout << "Alice" << endl;
        else cout << "Bob" << endl;
    else cout << "Alice" << endl;
    return 0;
}
```