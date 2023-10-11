---
title: 题解|G.Gcd-2023暑期牛客多校06
date: 2023/8/4
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 数学
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 9c670761
description: 题解|G.Gcd-2023暑期牛客多校06 Idx:1 数学
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

# G.Gcd
**数论**
## 题目大意
给定一个包含两个非负数的初始集合 $S=\{x,y\}$ 
每次操作可以选定其中不相等的两个数 $a,b$ ，并将 $a-b$ 或 $gcd(a,b)$ 置入集合 $S$ ，其中 $gcd(0,a)=a$
可以操作任意次，问能否使得集合 $S$ 包含非负数 $z$

### 前置知识点
[裴蜀定理](https://oi-wiki.org/math/number-theory/bezouts/)
## 解题思路
根据裴蜀定理，两个正整数辗转相减只能得到他们最大公约数的倍数//
因此对于 $z$ ，判断其是否是 $g=gcd(x,y)$ 的倍数即可。
如果 $z$ 是 $g$ 的倍数，则可以通过以下操作得到 $z$ ：
1. 将 $g=gcd(x,y)$ 置入集合
2. $x$ 作为 $g$ 的倍数，其加减任意次 $g$ 便可得到任意 $g$ 的倍数。
   只能减不能加怎么办呢//先把 $x$ 减到 $-g$ 就好了

值得注意的是，本题的数据约束为**非负数**，这意味着需要对 $0$ 的情况进行特判//
1. 对于 $z=0$ ，仅当 $x,y$ 有 $0$ 时有解
2. 对于 $x=0$ 或 $y=0$ ，仅当 $z$ 为非 $0$ 项的倍数时有解（实际上这条也满足裴蜀定理，直接归入一般情况即可）

参考样例：
```
in: 1 0 1 2
out: YES
```

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll x,y,z;
    cin >> x >> y >> z;
    if(x&&y&&z==0) {cout << NO;return;}
    ll g=gcd(x,y);
    if(z%g) cout << NO;
    else cout << YES;
}
```