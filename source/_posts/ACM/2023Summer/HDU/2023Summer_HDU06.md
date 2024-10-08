---
title: 题解|2023暑期杭电多校06
date: 2023/8/3
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_HDU06
description: 题解|2023暑期杭电多校06
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

# 1001.Count
**构造**
## 题意
给定 $n,m,k$ ，构造长度为 $n$ 的整数序列，元素大小范围为 $a_i\in [1,m]$ ，并且需要保证前 $k$ 个元素和后 $k$ 个元素对应相同
求可以构造出的序列数量

## 解题思路
模拟一下即可
对于 $k\le \dfrac{n}{2}$ ，最后 $k$ 个元素由前 $k$ 个元素确定，只需决定前 $n-k$ 个元素
对于 $\dfrac{n}{2}\le k \lt n$ ，确定了前 $n-k$ 个元素，可以递归向后确定更多元素
对于 $k=n$ ，序列本身自然相同，即可以随意确定 $n$ 个元素

确定 $x$ 个元素的方案总数为 $m^{x}$ ，快速幂取模即可
(P.S.):快速幂前先对底数取模// 

### 时间复杂度
快速幂： $O(\log n)$

## 参考代码
```cpp
void solve()
{
    ll n,m,k;
    cin >> n >> m >> k;
    if(n==k) cout << qcpow(m,n) << endl;
    else cout << qcpow(m,n-k) << endl;
}
```