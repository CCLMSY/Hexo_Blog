---
title: 动态规划-杂项
date: '2023/9/22 20:00'
categories: 笔记模板
keywords: 动态规划
description: 动态规划-杂项//一些不成类的题
tags:
  - 动态规划
  - 笔记
abbrlink: 4014d4be
updated:
sticky:
swiper_index:
---
# 一.子序列问题
## 1.最长公共子序列(LCS)
O(mn)
```cpp
#define N 1005
ll a[N]={0},b[N]={0},dp[N][N]={0};
void solve()
{
    ll m,n;cin >> n >> m;
    FORLL(i,1,n) cin >> a[i];
    FORLL(i,1,m) cin >> b[i];
    FORLL(i,1,n)
        FORLL(j,1,m){
            if(a[i]==b[j]) dp[i][j]=dp[i-1][j-1]+1;
            else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
        }
    cout << dp[n][m] << endl;
}
```

## 2.最长上升子序列
### 2.1.DP
O(n^2) 洛谷B3637
```cpp
/*----------Code Area----------*/
#define N 10005
ll a[N]={0},dp[N]={0};
void solve()
{
    ll n;cin >> n;
    FORLL(i,1,n) cin >> a[i];
    dp[1]=1;ll ans=0;
    FORLL(i,2,n){
        ll mx=0;
        FORLL(j,1,i-1)
            if(a[i]>a[j]) mx=max(mx,dp[j]);
        dp[i]=mx+1;
        ans=max(ans,dp[i]);
    }
    cout << ans << endl;
}
```

### 2.2.贪心
O(nlogn) 洛谷B3637
贪心：维护当前子序列d，替换序列中不小于a[i]的第一个元素
```cpp
#define N 10005
ll a[N]={0};
vector<ll> d;
void solve()
{
    ll n;cin >> n;
    FORLL(i,1,n) cin >> a[i];
    d.emplace_back(a[1]);
    FORLL(i,2,n){
        if(d.back()<a[i]) d.emplace_back(a[i]);
        else *lower_bound(ALL(d),a[i])=a[i];
    }//最长不降子序列改成upper_bound
    cout << d.size() << endl;
}
```

# 二.DP优化
## 1. 单调队列优化DP
O(mn) M(m) CF372C
利用单调队列将每次区间DP均摊复杂度降至O(1)
```cpp
#define N 305
#define M 150005
ll a[N]={0},b[N]={0},t[N]={0};
ll dp[2][M]={0};
ll n,m,d;int fl=1;
ll que[M]={0};
void solve()
{
    cin >> n >> m >> d;
    FORLL(i,1,m) cin >> a[i] >> b[i] >> t[i];
    fl=1;
    FORLL(i,1,m){
        ll l=1,r=0,k=1;
        FORLL(j,1,n){
            for(;k<=min(n,j+(t[i]-t[i-1])*d);k++){
                while(r>=l&&dp[fl^1][que[r]]<dp[fl^1][k]) r--;
                que[++r]=k;//单调队列优化DP：维护上一状态的有效区间内的最大值的下标
            }
            while(r>=l&&que[l]<max(1ll,j-(t[i]-t[i-1])*d)) l++;
            dp[fl][j]=dp[fl^1][que[l]]+b[i]-abs(a[i]-j);
        }fl^=1;//状态转换
    }fl^=1;//回到最终状态
    ll ans=-INF;
    FORLL(i,1,n) ans=max(ans,dp[fl][i]);
    cout << ans << endl;
}
```

Reference：[OI-Wiki](https://oi-wiki.org/dp/)