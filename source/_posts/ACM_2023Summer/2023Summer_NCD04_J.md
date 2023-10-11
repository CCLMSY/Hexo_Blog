---
title: 题解|J.Qu'est-ce Que C'est?-2023暑期牛客多校04
date: 2023/7/28
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 动态规划
  - 计数
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 9d48d78e
description: 题解|J.Qu'est-ce Que C'est?-2023暑期牛客多校04 Idx:4 动态规划/状态压缩
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

# J.Qu'est-ce Que C'est?
**动态规划**
## 题目大意
给定两个正整数 $n,m$ ，要求构造长度为 $n$ 的整数序列 $a$ ，满足：
1. $\forall i\in [1,n],-m\le a_i\le m$
2. 任意长度大于 $1$ 的连续子段之和不小于 $0$

求满足以上条件的整数序列的个数，结果取模
## 解题思路
计数取模，比起DP我会首先考虑猜通项
赛时根据样例1：$n=3,m=3\Rightarrow130$ 以及手算的 $n=2,m=3\Rightarrow28$ ，结合题目特征，拟出了一个满足上面两个情况的很奇怪的式子//然后样例2过不了，~~开摆~~

DP的思想是先计算出第 $i$ 个位置前已经满足题设条件，且最小后缀和为 $j$ 的方案数量（ $j$ 可以是负数，因为最后一个数可以单独为负数）
考虑如何进行状态转移

对每个位置从大到小遍历当前位置填数后的最小后缀和 $j$ ，记当前遍历到 $i$ ，填入的数为 $x$
1. 对于非负整数 $j$ ，填入 $x$ 时的方案数为 $dp_{i-1,j-x}$ 
其中 $j-x\in[-m,m],x\in[-m,m],j\ge 0 \Rightarrow x\in [j-m,m]$，此时 $dp_{i,j}=\sum\limits_{x=j-m}^m dp_{i-1,x}$
2. 对于负整数 $j$ ，此时 $x=j$ ，其方案数为允许 $x$ 填入的所有方案数之和，即 $dp_{i,j}=\sum\limits_{x=-j}^m dp_{i-1,x}$

由于每次计算都用到了前一个位置的以 $m$ 为上界的区间和，因此可以对每个位置的dp数组从 $m$ 到 $-m$ 维护一个前缀和，以降低时间复杂度

## 复杂度
时间复杂度： $O(n^2)$
空间复杂度： $M(Cn^2)$ ， $C$ 是常数，开LL可能会导致MLE
7/30：开L都MLE了//进行了空间优化，只保留 $dpsum$ 数组

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
#define O 5000 //记为0
long dp[5005][10005]={0};
//表示前i-1个数的最小后缀和为j+5000时，满足条件的可行方案数
long dpsum[5005][10005]={0};
//表示dp[i][m]到dp[i][j]可行方案数的区间和
void solve()
{
    ll m,n;
    cin >> n >> m;
    for(ll j=m;j>=-m;j--) {dp[1][j+O]=1;dpsum[1][j+O]=dpsum[1][j+1+O]+dp[1][j+O];}
    //预处理前0位
    FORLL(i,2,n){
        for(ll j=m;j>=-m;j--){
            if(j>=0) dp[i][j+O]=dpsum[i-1][j-m+O];
            //下界x+m=j => x=j-m
            else dp[i][j+O]=dpsum[i-1][-j+O];
            //j<0 下界-j
            dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dp[i][j+O]);
            //计数取模
        }
    }cout << dpsum[n][-m+O] << endl;
}
```
7/30：空间优化后
```cpp
#define O 5000 //记为0
//long dp[5001][10001]={0};//表示前i-1个数最小后缀和为j+5000，满足条件的可行方案数
long dpsum[5001][10001]={0};//表示dp[i][m]到dp[i][j]可行方案数的区间和
void solve()
{
    ll m,n;
    cin >> n >> m;
    for(ll j=m;j>=-m;j--){
        //dp[1][j+O]=1;
        //dpsum[1][j+O]=dpsum[1][j+1+O]+dp[1][j+O];
        dpsum[1][j+O]=dpsum[1][j+1+O]+1;
    }
    FORLL(i,2,n){
        for(ll j=m;j>=-m;j--){
            //if(j>=0) dp[i][j+O]=dpsum[i-1][j-m+O];//下界x+m=j => x=j-m
            if(j>=0) dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dpsum[i-1][j-m+O]);
            //else dp[i][j+O]=dpsum[i-1][-j+O];//j<0 下界-j
            else dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dpsum[i-1][-j+O]);
            //dpsum[i][j+O]=Get_Mod(dpsum[i][j+1+O]+dp[i][j+O]);
            
        }
    }cout << dpsum[n][-m+O] << endl;
}
```