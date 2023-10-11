---
title: 题解|J.Roulette-2023暑期牛客多校01
date: 2023/7/17
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 概率/期望
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 1e003c09
description: 题解|J.Roulette-2023暑期牛客多校01 Idx:2 概率/期望
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

# J.Roulette

**数学题**

## 题目大意
初始有 $n$ 元钱，目标为赢得 $m$ 元钱（即共 $n+m$ 元钱）
每次投注 $x$ 元，有50%的概率输掉，50%的概率赢得 $2x$ 元
第一局投注 $1$ 元，接下来的每局按下述策略投注：
$$\begin{cases}
    x_i=1 , \text{ 上一局赢了} \newline
    x_i=2 \times x_{i−1} ， \text{ 上一局输了}
\end{cases}$$
如果某局剩下的钱不足以按上述策略投注，则失败退场
问达成目标的概率有多大

## 解题思路
投注采用的是**倍投法**，考虑每一轮投注为：输若干局直到赢一局
1. 第一局就赢了，则获得 $1$ 元
2. 第 $n$ 局赢了，则获得 $2^{n-1} - (2^{n-2}+2^{n-3}+\cdots+2+1)=1$ 元 

综上，**每一轮赢取的钱数固定为 $1$ 元**

根据题意，直到输到无法投注才失败，则对于每轮投注
1. 失败的概率为： $\dfrac{1}{2^a}$ 
2. 成功的概率为： $1-\dfrac{1}{2^a}$

其中 $a$ 是这轮投注的局数
那么对于整个投注过程，成功的概率为：$\prod\limits_{i=n}^{n+m-1} (1-\dfrac{1}{2^{a_i}})$


考虑本金与可以进行的投注轮数的关系，进行第a轮至少需要的本金为：
$1+2+\cdots+2^{a-1}=2^a-1$
以此为边界对集合 $\{n,n+1,\cdots,m+n-1\}$ 做划分（划分内投注轮数相同，成功率相同），分别计算每个划分的成功率，再相乘得到结果

### 时间复杂度
划分数 $\log n$，快速幂等操作 $\log n$，总复杂度 $O(\log^2n)$

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
const int MOD=998244353;
#define Get_Mod(x) ((x)%MOD)
ll QuickPow(ll x,ll y){
    ll ans=1;
    while(y){
        if(y&1) ans=Get_Mod(x*ans);
        x=Get_Mod(x*x);
        y>>=1;
    }
    return ans;
}
ll Get_Inv(ll a){
    return QuickPow(a,MOD-2);
}
int solve()
{
    ll n,m;
    cin >> n >> m;
    ll a,b,c;
    ll base=0,t=(n+1)>>1;
    //base存储当前划分的投注轮数
    while(t){
        base++;
        t>>=1;
    }
    ll l=n,r=pow(2,base+1)-2;
    //l,r分别记录当前划分的左右边界
    ll re=1;
    if(m+n<=r)//所有操作数都在一个划分中，则m+n-1作为右边界
    {
        a=QuickPow(2,base);
        b=Get_Mod(Get_Mod(a+MOD-1)*Get_Inv(a));
        //b为等比数列求和
        c=QuickPow(b,m);//这个划分共m局游戏
        re=Get_Mod(re*c);
        base++;
    }
    else{
        while(r<=m+n-1)
        {
            a=QuickPow(2,base);
            b=Get_Mod(Get_Mod(a+MOD-1)*Get_Inv(a));
            c=QuickPow(b,r-l+1);//完全的划分共r-l+1局游戏
            re=Get_Mod(re*c);

            base++;
            l=pow(2,base)-1;
            r=pow(2,base+1)-2;
        }
        if(l<m+n)
        {
            a=QuickPow(2,base);
            b=Get_Mod(Get_Mod(a+MOD-1)*Get_Inv(a));
            c=QuickPow(b,m+n-l);//最后一个的划分共m+n-l局游戏
            re=Get_Mod(re*c);
        }
    }
    cout << re << endl;
    return 0;
}
```