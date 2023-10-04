---
title: 动态规划（DP）
date: '2023/9/22 20:00'
updated: 2023/10/4 12：00
categories: 笔记模板
keywords: 动态规划
description: 动态规划部分例题、板子代码
tags:
  - 动态规划
  - 笔记
sticky: 10
swiper_index: 2
abbrlink: '879e7329'
---

# 动态规划（DP）

## 一.子序列问题
### 1.最长公共子序列(LCS)
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

### 2.最长上升子序列
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

### 3.最长上升子序列
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

## 二.背包DP
### 1.DFS记忆化搜索
O(mn) 洛谷P1048
```cpp
#define N 1005
#define M 105
ll v[N]={0},w[N]={0};
ll mem[M][N]={0};
ll maxw,n;
ll dfs(ll i,ll curw){
    if(mem[i][curw]) return mem[i][curw];
    if(i>n) return mem[i][curw]=0;
    if(curw>=w[i])
        return mem[i][curw]=max(dfs(i+1,curw-w[i])+v[i],dfs(i+1,curw));
    else 
        return mem[i][curw]=dfs(i+1,curw);
}
void solve()
{
    cin >> maxw >> n;
    FORLL(i,1,n) cin >> w[i] >> v[i];
    cout << dfs(1,maxw) << endl;
}
```

### 2.01背包二维数组
O(wn) M(wn) 洛谷P1048
```cpp
/*----------Code Area----------*/
#define N 1005
#define W 105
ll v[N]={0},w[N]={0};
ll dp[N][W]={0};
ll n,maxw;
void solve()
{
    cin >> maxw >> n;
    FORLL(i,1,n) cin >> w[i] >> v[i];
    FORLL(i,1,n)
        FORLL(j,0,maxw){
            if(j>=w[i]) dp[i][j]=max(dp[i-1][j-w[i]]+v[i],dp[i-1][j]);
            else dp[i][j]=dp[i-1][j];
        }
    cout << dp[n][maxw] << endl;
}
```

### 3.01背包一维数组
O(wn) M(w) 洛谷P2871
一维滚动数组空间优化
```cpp
#define N 5005
#define W 20005
ll v[N]={0},w[N]={0};
ll dp[W]={0};
ll n,maxw;
void solve()
{
    cin >> n >> maxw;
    FORLL(i,1,n) cin >> w[i] >> v[i];
    FORLL(i,1,n) 
        FORLL_rev(j,maxw,w[i]){//01背包从大到小遍历，每个物品只能取一次
            dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
        }
    cout << dp[maxw] << endl;
}
```

### 4.完全背包一维数组
O(wn) M(w) 洛谷P1616
一维滚动数组空间优化，和01背包唯一区别在剩余容量从小到大遍历，每个物品能取多次
```cpp
#define N 10005
#define W 10000005
ll v[N]={0},w[N]={0};
ll dp[W]={0};
ll n,maxw;
void solve()
{
    cin >> maxw >> n;
    FORLL(i,1,n) cin >> w[i] >> v[i];
    FORLL(i,1,n) 
        FORLL(j,w[i],maxw){//和01背包唯一区别在从小到大遍历，每个物品能取多次
            dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
        }
    cout << dp[maxw] << endl;
}
```

### 5.多重背包
$O(w\sum\limits\lg{cnt_i})$
朴素想法：按有 $cnt_i$ 个的物品 $i$ 进行01背包
二进制分组优化：对于每个物品，将其按二进制分组，捆绑一个物品
```cpp
#define N 10005
#define W 10000005
ll v[N]={0},w[N]={0},cnt[N]={0};
ll dp[W]={0};
ll n,maxw;
void solve()
{
    cin >> maxw >> n;ll tn=n,t,b;
    FORLL(i,1,n) cin >> w[i] >> v[i] >> cnt[i];
    FORLL(i,1,tn){//二进制分组
        t=cnt[i];b=2;
        while(t>b){
            v[++n]=v[i]*b;
            w[n]=v[i]*b;
            t-=b;b*=2;
        }
        if(t) {v[++n]=v[i]*t;w[n]=v[i]*t;}
    }
    FORLL(i,1,n) 
        FORLL_rev(j,maxw,w[i]){
            dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
        }
    cout << dp[maxw] << endl;
}
```

## 三.DP优化
### 1. 单调队列优化DP
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