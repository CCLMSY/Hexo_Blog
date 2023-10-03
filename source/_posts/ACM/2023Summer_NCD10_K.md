---
title: 题解|K.First Last-2023暑期牛客多校10
date: 2023/8/18
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 概率/期望
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: f6ff419e
description: 题解|K.First Last-2023暑期牛客多校10  Idx:1  概率/期望
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

# K.First Last
**签到题**
## 题目大意
$n$ 个人参加 $m$ 场比赛，每场比赛中获得名次得概率均等
问针对某一人，他在所有场次比赛中都获得第一或倒数第一的概率

## 解题思路
如果人数 $n>1$ ，每场比赛的概率是 $p=\dfrac{2}{n}$ ；人数为 $1$ 时 $p=1$

输出 $p^m$ 即可


## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll m,n;cin >> n >> m;
    double p=2./n,re=1;
    if(n==1) p=1;
    FORLL(i,1,m) re*=p;
    print_float(re,15);cout << endl;
}
```