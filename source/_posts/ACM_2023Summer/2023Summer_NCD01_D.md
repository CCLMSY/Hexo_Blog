---
title: 题解|D.Chocolate-2023暑期牛客多校01
date: 2023/7/17
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
abbrlink: df10473b
description: 题解|D.Chocolate-2023暑期牛客多校01 Idx:1 博弈
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

## 题目大意
有一块大小为n*m的巧克力，起始坐标为(1,1)

每次操作可以选择一个坐标(a,b)，并吃掉(1,1):(a,b)中的所有巧克力

每次操作必须吃掉至少一块巧克力，吃掉最后一块巧克力的玩家输

## 解题思路
**1\*1**：显然先行必输

**其他**：对于总数m\*n。若总数为奇数，则先行开局吃偶数个，若总数为偶数，则先行开局吃奇数个。
然后剩余个数变为奇数个，后续每一步，先行者都保证吃完后剩余奇数个巧克力且行数/列数多于1,或1\*1（可以证明在任何状态下，一定有一种这样的操作）。

先行必胜

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

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
