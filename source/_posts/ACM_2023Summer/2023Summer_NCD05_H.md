---
title: 题解|H.Nazrin the Greeeeeedy Mouse-2023暑期牛客多校05
date: 2023/7/31
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 动态规划
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 19eda18c
description: 题解|H.Nazrin the Greeeeeedy Mouse-2023暑期牛客多校05 Idx:4 动态规划/背包DP
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

# H.Nazrin the Greeeeeedy Mouse
**动态规划**
## 题目大意
给定一组 $n$ 个奶酪，第 $i$ 个奶酪具有重量 $w_i$ 和价值 $val_i$
将前来取 $m$ 次奶酪，每次会携带一个大小为 $sz$ 的背包，并保证每一次携带背包的大小不小于前一次，并从第一个奶酪出发
对于每一个奶酪，可以选择装入背包（如果背包剩余空间足够）或挖洞通过（被挖洞的奶酪无法带走），并且仅当上一块奶酪被带走或挖洞，才能到达下一块
每次出发的过程不独立，问 $m$ 次可以获得最大价值为多少//

## 解题思路
由于每次携带的背包大小都不小于前一次，因此如果取的次数 $m$ 多于奶酪个数 $n$ ，只需要考虑最后 $n$ 个背包即可//感性证明一下：如果倒数 $n$ 背包内某次装不下任何一块奶酪，那么之前的背包也不可能装下//

对于奶酪，只有取和不取两种状态（为不影响后续操作，没取的奶酪直接视为挖洞）
对于某一轮行动，假设最优解下它取的第一个奶酪和最后一个奶酪之间的区间为 $[l,r]$ ，那么可以视为这一轮行动是携带空间为 $sz_i$ ，第 $[l,r]$ 个奶酪上的01背包问题

由于不确定每轮取的范围以及对应背包大小，因此每个区间的每个重量都需要一个01背包DP值
用 $dp_{i,j,k}$ 表示在区间 $[i,j]$ 内，取重量为 $k$ 的物品可以获得的最大价值，可以得到以下状态转移方程：
$$
dp_{i,j,k}=\begin{cases}
    dp_{i,j-1,k} &,dp_{i,j-1,k-w[j]}+val_j<dp_{i,j-1,k}或 k<w_j\newline
    dp_{i,j-1,k-w[j]}+val_j &,dp_{i,j-1,k-w[j]}+val_j>dp_{i,j-1,k}
\end{cases}
$$

在计算完 $[i,j]$ 内的 $dp$ 值后，前缀维护每一个 $k$ 的 $dp_{i,j,k}$ ，使得对于该区间， $dp$ 值随 $k$ 增加不递减

最后对全部 $m$ 轮行动进行DP，记 $g_{i,j}$ 为第 $i$ 次行动携带背包的容量为 $sz$ ，到达的最后一个位置为 $j$ 所得到的最大价值，可以得到以下状态转移方程：
$$
g_{i,j}=max\{g_{i-1,k}+dp_{k+1,j,sz}\},k\in[1,j-1]
$$


### 时间复杂度
$O(n^2w)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
#define N 205
ll dp[N][N][N]={0};
ll w[N]={0},val[N]={0};
ll g[N][N]={0};
void solve()
{
    RESET(dp);
    ll n,m,t;
    cin >> n >> m;
    FORLL(i,1,n) cin >> w[i] >> val[i];
    //规划[i,j]区间内取体积为k的最优解
    FORLL(i,1,n){//从第i个开始考虑
        FORLL(j,i,n){//目前考虑第j个取/不取
            FORLL(k,0,w[j]-1) dp[i][j][k]=dp[i][j-1][k];
            //对于无法装下第j个的情况，用j-1之前的维护
            FORLL(k,w[j],N) dp[i][j][k]=max(dp[i][j-1][k],dp[i][j-1][k-w[j]]+val[j]);
            //可以装下的情况，更新较大值
        }
        FORLL(j,1,n)
            FORLL(k,1,n)
                dp[i][j][k]=max(dp[i][j][k],dp[i][j][k-1]);
                //对于区间[i,j]，前缀维护最优解
    }
    if(m>n){FORLL(i,1,m-n) cin >> t; m=n;}ll sz,ans=0;
    FORLL(i,1,m){cin >> sz;
        FORLL(j,1,n){
            FORLL(k,0,j-1) g[i][j]=max(g[i][j],g[i-1][k]+dp[k+1][j][sz]);
            //第二项：前i个背包最远拿到第k项+第i个背包拿[k+1,j]内
            ans=max(ans,g[i][j]);
        }
    }
    cout << ans << endl;
}
```