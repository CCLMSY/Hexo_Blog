---
title: 题解|D.Ama no Jaku-2023暑期牛客多校03
date: 2023/7/24
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
abbrlink: 4b1f70fb
description: 题解|D.Ama no Jaku-2023暑期牛客多校03 Idx:3 思维·矩阵
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

# D.Ama no Jaku
**思维**
## 题目大意
给定一个01矩阵，每次操作可以选定任意一行或一列并将其反转
问在操作任意次后，能否使得该矩阵 行代表的二进制数$r_i$ 与列代表的二进制数 $c_i$ ，满足 $min(r_i)\ge max(c_i)$
若能，输出最少操作次数，否则输出“-1”

## 解题思路
首先考虑满足条件的矩阵的状态
对于 $\text O$ 矩阵，$\forall i ,r_i=c_i=0$ ，一定满足条件
要使 $min(r_i)>0$ ，每一行都要有 $1$ 。如果在每一行都加上一个 $1$ ，会发现无论如何排列顺序，都会使得 $min(r_i)\lt max(c_i)$
最终当所有元素都转化为 $1$ 时，满足条件
综上，只有当元素全为 $0$ 或 $1$ 时满足条件

显然如果一个矩阵可以转化为全 $0$ 矩阵，那么它一定可以转化为全 $1$ 矩阵，因此考虑原矩阵是否可以转化为全 $0$ 矩阵

先将第一列全部转化为 $0$ ，后面每一列的值都必须全为 $0$ 或 $1$ 才可以满足条件
分别记录行和列的首位转化为0的次数，分类比较即可

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll n,cntr=0,cntc=0,re1,re2;//转置为0的次数
    cin >> n;
    bitset<2005> mat[2005];
    FORLL(i,0,n-1) cin >> mat[i];
    if(n==1) {cout << 0 << endl;return;}
    FORLL(i,0,n-1) if(mat[i][0]) {mat[i].flip();cntr++;}
    //FORLL(i,0,n-1) {FORLL(j,0,n-1) cout << mat[i][j]; cout << endl;}
    FORLL(j,1,n-1){
        int b=mat[0][j];
        if(b) cntc++;
        FORLL(i,1,n-1) if(mat[i][j]!=b) {cout << "-1" << endl;return;}
    }
    re1=cntc+(cntr>=(n+1)/2?cntr:n-cntr);
    re2=cntr+(cntc>=(n+1)/2?cntc:n-cntc);
    cout << (re1<re2?re1:re2) << endl;
}
```