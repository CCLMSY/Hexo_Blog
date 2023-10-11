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

# 二.背包DP
## 1.01背包
### 1.1.DFS记忆化搜索
O(mn) 洛谷P1048
```cpp
#define N 1005
#define W 105
ll v[N]={0},w[N]={0};
ll mem[W][N]={0};
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

### 1.2.二维数组
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

### 1.3.一维滚动数组
O(wn) M(w) 洛谷P2871
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

## 2.完全背包一维数组
O(wn) M(w) 洛谷P1616
和01背包唯一区别在剩余容量从小到大遍历，每个物品能取多次
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

## 3.多重背包
### 3.1.朴素方法
$O(w\sum cnt_i)$
朴素方法：按有 $cnt_i$ 个的物品 $i$ ，进行01背包
```cpp
#define N 100005
#define W 100005
ll v[N]={0},w[N]={0};
ll dp[W]={0};
ll n=0,maxw,tn;
void solve()
{
    cin >> tn >> maxw;
    ll tv,tw,cnt,n=0;
    FORLL(i,1,tn){
        cin >> tv >> tw >> cnt;
        FORLL(j,1,cnt){
            v[++n]=tv;
            w[n]=tw;
        }
    }
    FORLL(i,1,n)
        FORLL_rev(j,maxw,w[i])
            dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
    cout << dp[maxw] << endl;
}
```

### 3.2.二进制分组
$O(w\sum\lg{cnt_i})$
二进制分组优化：对于每个物品，将其按二进制分组，捆绑一个物品
```cpp
#define N 100005
#define W 100005
ll v[N]={0},w[N]={0};
ll dp[W]={0};
ll n=0,maxw,tn;
void solve()
{
    cin >> tn >> maxw;
    ll tv,tw,cnt,n=0;
    FORLL(i,1,tn){
        cin >> tv >> tw >> cnt;
        ll b=1;
        while(cnt>b){
            v[++n]=tv*b;
            w[n]=tw*b;
            cnt-=b;b*=2;
        }
        if(cnt) {v[++n]=tv*cnt;w[n]=tw*cnt;}
    }
    FORLL(i,1,n)
        FORLL_rev(j,maxw,w[i])
            dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
    cout << dp[maxw] << endl;
}
```

## 4.混合背包
$O(wn)$ 洛谷P1833
01背包、多重背包和完全背包的缝合怪//
```cpp
#define N 100005
#define W 100005
ll v[N]={0},w[N]={0};
bool flag[N]={0};
ll dp[W]={0};
ll n=0,maxw,tn;
void solve()
{
    ll h1,m1,h2,m2;char tc;
    cin >> h1 >> tc >> m1 >> h2 >> tc >> m2 >> tn;
    maxw=abs((h2*60+m2)-(h1*60+m1));
    ll tv,tw,cnt,n=0;
    FORLL(i,1,tn){
        cin >> tw >> tv >> cnt;
        if(cnt){
            ll b=1;
            while(cnt>b){
                v[++n]=tv*b;
                w[n]=tw*b;
                flag[n]=1;
                cnt-=b;b*=2;
            }//多重背包二进制分组
            if(cnt) {v[++n]=tv*cnt;w[n]=tw*cnt;flag[n]=1;}
        }else{
            v[++n]=tv;
            w[n]=tw;
            flag[n]=0;
        }
    }
    FORLL(i,1,n)
        if(flag[i]){//01背包
            FORLL_rev(j,maxw,w[i])
                dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
        }else{//完全背包
            FORLL(j,w[i],maxw)
                dp[j]=max(dp[j-w[i]]+v[i],dp[j]);
        }
    cout << dp[maxw] << endl;
}
```

## 5.二维费用背包
$O(nw_1w_2)$ 洛谷P1855
具有两种费用属性的背包问题，以01背包为例
```cpp
#define N 105
#define W 205
ll v[N]={0},w1[N]={0},w2[N]={0};
ll dp[W][W]={0};//二维滚动数组
ll n,maxw1,maxw2;
void solve()
{
    cin >> n >> maxw1 >> maxw2;
    FORLL(i,1,n){
        cin >> w1[i] >> w2[i];
        v[i]=1;//本题中价值均为1
    }
    FORLL(i,1,n)
        FORLL_rev(j1,maxw1,w1[i])
        FORLL_rev(j2,maxw2,w2[i])
            dp[j1][j2]=max(dp[j1-w1[i]][j2-w2[i]]+v[i],dp[j1][j2]);
    cout << dp[maxw1][maxw2] << endl;
}
```

## 6.分组背包

# 三.DP优化
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