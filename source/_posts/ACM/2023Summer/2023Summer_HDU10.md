---
title: 题解|2023暑期杭电多校10
date: 2023/8/17
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_HDU10
description: 题解|2023暑期杭电多校10
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

# 1004.Do You Like Interactive Problems?
**概率论-数学期望**
## 题目大意
给定一个正整数 $n$ ，在 $1$ ~ $n$ 范围内有一个正整数 $x$ 
进行若干轮猜测，每次猜测在 $1$ ~ $n$ 范围内等概率随机选择一个正整数 $y$ ，可以得到如下信息中的一个： $y>x,y=x,y<x$ 
**每次猜测后，下一次猜测仍在原范围内随机选择**。当得到的信息能够唯一确定 $x$ 时，游戏结束
求猜测轮数的数学期望

## 解题思路
由题意可知，当且仅当出现以下情况时，可以唯一确定 $x$ ：
1. 选到 $x$ 
2.  $x$ 的相邻元素都被选到

每次选择后，下一次选择的范围是不变的，因此其他元素的信息对唯一确定 $x$ 是没有作用的

对于给定 $n$ ，根据以下情况展开讨论：
1. 不论 $x$ 的位置，直接选到 $x$ ，轮数为 $1$ ，概率为 $\dfrac{1}{n}$ 
2. 已经选到一个相邻点（或 $x$ 在两端），轮数期望记为 $E_2$
   1. 选另一相邻点（或唯一相邻点），轮数为 $1$ ，概率为 $\dfrac{1}{n}$ 
   2. 选其他点没有贡献，概率为 $\dfrac{n-2}{n}$ 
3. $x$ 在中间，没有选到过相邻点，轮数期望记为 $E_3$
   1. 选到两个相邻点之一，转移到情况 $2$ ，概率为 $\dfrac{2}{n}$ 
   2. 选到其他点没有贡献，概率为 $\dfrac{n-3}{n}$

综上可得：

$E_2=\dfrac{1}{n}+\dfrac{1}{n}+\dfrac{n-2}{n}(1+E_2)$

$E_3=\dfrac{1}{n}+\dfrac{2}{n}(1+E_2)+\dfrac{n-3}{n}(1+E_3)$

解得 $E_2=\dfrac{n}{2},E_3=\dfrac{2n}{3}$
得到最终期望为：$E=\dfrac{2}{n}E_2+\dfrac{n-2}{n}E_3=\dfrac{2n-1}{3}$

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    if(n==1) {cout << 0 << endl;return ;}
    ll t=mul(Get_Mod(2*(n-2)+3),inv(3));
    cout << t << endl;
}
```

***

# 1012.Equalize the Array
**签到**
## 题目大意
定义一个数组的 $mode$ 是其中出现次数最多的数字（可不唯一）
给定一个数组 $a$ ，每次操作可以选定其一个 $mode$ 并使数组中所有与之相等的元素 $+1$
问任意次操作后能否使数组中全部元素相等

## 解题思路
如果数组中最小的元素是 $mode$ ，连续操作最小元素即可实现//
元素计数，判断最小元素出现次数是否最多即可

### 时间复杂度
$O(n\log n)$

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    map<ll,ll> mp;ll t;
    FORLL(i,1,n){
        cin >> t;mp[t]++;
    }
    ll mxcnt=0,mxi=0,mn=0;
    FORLL(i,1,n){
        t=mp[i];if(!mn&&t) mn=i;
        if(t>mxcnt) {mxcnt=t;mxi=i;}
    }
    if(mxi<=mn) cout << YES;
    else cout << NO;
}
```