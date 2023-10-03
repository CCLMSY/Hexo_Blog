---
title: 题解|A.World Fragments I-2023暑期牛客多校03
date: 2023/7/24
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 60ea0959
description: 题解|A.World Fragments I-2023暑期牛客多校03 Idx:1 签到
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

# A.World Fragments I
**字符串、签到**
## 题目大意
给定两个二进制数 $x,y$ 
每次可以选择 $x$ 中的一个数位 $d$ ,并可执行 $x=x+d$ 或 $x=x-d$
问能否通过任意次以上操作，使得 $x=y$ ，并给出最少次数

## 解题思路
这两个二进制数可以视为01串
对于 $x=0$ ，每一位都是0，除非 $y=0$ ，否则无法达成条件
对于 $x\ne 0$ ，必定有一位上是 $1$ ，通过题设操作可以转化为任何数，最小次数为这两个数的差的绝对值 $|x-y|$

bitset是一种非常便于处理01串的STL数据结构，合理利用可以省去很多麻烦

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    string x,y;
    cin >> x >> y;
    bitset<1000> a(x),b(y);
    ll m,n;
    if(a.none()&&b.any()) {cout << "-1" << endl;return;}
    m=a.to_ullong();
    n=b.to_ullong();
    cout << abs(m-n) << endl;
}
```