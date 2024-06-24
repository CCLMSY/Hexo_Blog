---
title: 题解|2023暑期牛客多校05
date: 2023/7/31
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_NCD05
description: 题解|2023暑期牛客多校05
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

# C.Cheeeeen the Cute Cat
**二部图最大匹配**
## 题目大意
给定一个具有 $2n$ 个节点的二部图，前 $n$ 个节点和后 $n$ 个节点各成一部
对于每对 $(i,j),i\ne j$ ，保证在 $i,j+n$ 和 $j,i+n$ 之间有且仅有一条边，整幅图的边数共 $C_n^2$ 条
求这个二部图的最大匹配

### 前置知识点
[二分图](https://oi-wiki.org/graph/bi-graph/)
[二分图最大匹配](https://oi-wiki.org/graph/graph-matching/bigraph-match/)

## 解题思路
题解和讨论区谈及竞赛图以及相关特点，稍微了解了一下但是赛事确实没发现可以把这个图转化成一个竞赛图//
当时一眼看去就像一道朴实无华的板子题，队友直接交了一发板子就过了//
跑一遍增广路求出最大匹配即可，具体的实现原理在OI-Wiki上有非常详细的推导与说明//
本蒟蒻就是看它学的qwq这里就不过多赘述了

## 参考代码
```cpp
void solve()
{
    ll n,t;
    cin >> n;
    augment_path G(n,n*2);
    FORLL(i,0,n-1)
        FORLL(j,0,n-1){
            cin >> t;
            if(t) G.add(i,j+n);
        }cout << G.solve() << endl;
}
```

***

# D.Cirno's Perfect Equation Class
**签到**
## 题目大意
给定三个整数 $k,c,n$ 
求满足以下条件的有序对 $(a,b)$ 的个数： $ka+b=c$ ， $b|c$ ， $gcd(a,b)>n$
## 解题思路
注意到第二个约束条件： $b$ 是 $c$ 的因子
因此对 $b$ 枚举，判断计数即可
### 时间复杂度
$O(qc^\frac{1}{2})$
## 参考代码
```cpp
void solve()
{
	ll k,c,n,a,tb;
	cin >> k >> c >> n;
	ll r=floor(sqrt(c)),cnt=0;
	for(ll b=1;b*b<=c;b++){
		if(c%b==0&&(c-b)%k==0){
			a=(c-b)/k;
			if(a&&b&&gcd(a,b)>=n) cnt++;
		}
		tb=c/b;
		if(tb!=b&&c%tb==0&&(c-tb)%k==0){
			a=(c-tb)/k;
			if(a&&tb&&gcd(a,tb)>=n) cnt++;
		}
	}cout << cnt << endl;
}
```

***

# G.Go to Play Maimai DX
**签到**
## 题目大意
给定一个长度为 $n$ 仅含有 $1,2,3,4$ 四种数字的序列
求最短的包含 $1,2,3$ 和 $k$ 个 $4$ 的区间的长度
## 解题思路
用快慢指针确定区间
快指针每次记录元素，直到满足条件；
慢指针每次移除元素，直到不满足条件
即可得到每个满足条件的最小区间，比较长度即可
### 时间复杂度
$O(n)$
## 参考程序
```cpp
#define N 100005
int v[N]={0};
void solve()
{
	ll n,k;cin >> n >> k;
    FORLL(i,1,n) cin >> v[i];
    ll l=0,r=0,cnt[5]={0},re=n;
    while(r<=n){
        while(!(cnt[1]&&cnt[2]&&cnt[3]&&cnt[4]>=k)){
            r++;cnt[v[r]]++;if(r>n) break;
        }
        while(cnt[1]&&cnt[2]&&cnt[3]&&cnt[4]>=k){
            l++;cnt[v[l]]--;if(l>r) break;
        }re=min(re,r-l+1);
    }cout << re << endl;
}
```

***

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