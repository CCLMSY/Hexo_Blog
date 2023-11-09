---
title: 题解|2023暑期牛客多校07
date: 2023/8/7
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 179d3b5a
description: 题解|2023暑期牛客多校07
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

# M.Writing Books
**签到**
## 题目大意
给定一个正整数 $n$ ，求 $1$ ~ $n$ 共有多少位数字

## 解题思路
$1$ ~ $9$ 共 $9\times 10^0 \times 1$ 位；
$10$ ~ $99$ 共 $9\times 10^1 \times 2$ 位；
以此类推，求和即可

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll n;
    cin >> n;
    ll a=1,x=9,cnt=0;
    while(n>=x) {n-=x;cnt+=a*x;x*=10;a++;}
    cnt+=a*n;
    cout << cnt << endl;
}
```