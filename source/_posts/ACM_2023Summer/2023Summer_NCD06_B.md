---
title: 题解|B.Distance-2023暑期牛客多校06
date: 2023/8/4
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 思维
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: cc1462ed
description: 题解|B.Distance-2023暑期牛客多校06 Idx:4 思维
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

# B.Distance
**数学、贪心(?)**
## 题目大意
对于两个大小相同的多重集 $\mathbb{A},\mathbb{B}$ ，可以选择其中任一元素 $x$ 执行操作 $x=x+1$  任意次数，最少的使得 $\mathbb{A},\mathbb{B}$ 相同的操作次数记为 $C(\mathbb{A},\mathbb{B})$ 
不同大小的 $\mathbb{A},\mathbb{B}$ 视为 $C(\mathbb{A},\mathbb{B})=0$

现在，给定两个大小为 $n$ 的多重集 $\mathbb{S},\mathbb{T}$ ，求对于 $\mathbb{S},\mathbb{T}$ 的所有子集 $\mathbb{A},\mathbb{B}$ ，最少操作次数之和 $\sum\limits_{\mathbb{A} \subseteq \mathbb{S}}\sum\limits_{\mathbb{B} \subseteq \mathbb{T}} C(\mathbb{A},\mathbb{B})$ 的值
具有相同值的两个元素视为不同元素，答案取模

## 解题思路
对于某对子集 $\mathbb{A},\mathbb{B}$ ，为了使他们相同的操作次数最少，我们会将他们排序的元素后一一对应，使每一对中较小的数变成较大的数//假设 $a_i$ 与 $b_i$ 对应，他们在这次变化中贡献的操作次数显然是 $|a_i-b_i|$ 

那么换一种角度考虑，对于原多重集 $\mathbb{S},\mathbb{T}$ ，任取一对数 $a_i,b_j$ ，考虑它们俩对应的方案数 $cnt_{i,j}$ ，那么它们在全部方案中贡献的总操作次数即为 $|a_i-b_i|\times cnt_{i,j}$

由于我们的操作策略是排序后对应，因此先对 $\mathbb{S},\mathbb{T}$ 进行排序//
选定两个数 $a_i,b_j$ 后，它们在 $\mathbb{S},\mathbb{T}$ 中的位置前面选 $k$ 对数的方案数为 $\sum\limits_{k=0}^{min(i-1,j-1)}C_{i-1}^kC_{j-1}^k=C_{i+j-2}^k$ （[范德蒙德卷积](https://oi-wiki.org/math/combinatorics/vandermonde-convolution/)）

同理，它们在 $\mathbb{S},\mathbb{T}$ 中的位置后面选 $k$ 对数的方案数为 $C_{2*n-i-j}^k$
总方案数为 $cnt_{i,j}=C_{i+j-2}^kC_{2*n-i-j}^k$ ，乘以两数之差的绝对值即为它们对答案的总贡献//

预处理组合数，枚举 $i,j$ 求和即可

### 时间复杂度
$O(n^2)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
#define N 2005
void solve()
{
    ll n,t;cin >> n;
    vector<ll> a(n),b(n);
    for(auto &x:a) cin >> x;
    for(auto &x:b) cin >> x;
    ll re=0;SORT(a);SORT(b);
    FORLL(i,0,n-1) FORLL(j,0,n-1)
        addto(re,mul(abs(a[i]-b[j]),mul(Get_Combination(i+j,i),Get_Combination((n-i-1)+(n-j-1),(n-i-1)))));
    cout << re << endl;
}
```