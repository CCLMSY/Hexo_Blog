---
title: 题解|2024牛客寒假04
tags:
  - 2024牛客寒假
categories: 算法竞赛
mathjax: true
abbrlink: ACM_2024Winter_NCD04
date: 2024/2/19
description: 题解|2024牛客寒假04
---
# A.柠檬可乐
**1签**
## 题意
第一行输入三个整数$a,b,k$，判断是否$a\ge k\times b$

## 解题思路
如题

## 参考程序
```cpp
void solve()
{
    ll a,b,k;
    cin >> a >> b >> k;
    if(a>=b*k) cout << "good\n";
    else cout << "bad\n";
}
```

# B.左右互博
**思维**
## 题意
有$n$堆石子，每堆石子有$a_i$个，两个人轮流取操作。
每次操作可以选择一堆至少有$2$个石子的石子堆，然后任意分成两堆，每堆至少有一个石子。
最先无法操作的人输。

## 解题思路
对于有$x$个石子的石堆，它被分成$x$堆一个石子的石堆，需要操作$x-1$次。
因此，对所有石堆的操作次数求和 $sum = \sum\limits_{i=1}^{n}a_i-1$。
判断奇偶性

## 参考程序
```cpp
void solve()
{
    ll n,t;cin >> n;
    ll ans=0;
    FORLL(i,1,n){
        cin >> t;
        ans+=t-1;
    }
    if(ans%2) cout << "gui" << endl;
    else cout << "sweet" << endl;
}
```

# C.冬眠
**模拟**
## 题意
给定$n\times m$的字符矩阵和一段有$q$次操作的操作序列，包含两种操作：
1. `1 t`表示将第$t$行向右移动$1$次，最右的一个字符移到最左的位置。
2. `2 t`表示将第$t$列向下移动$1$次，最下的一个字符移到最上的位置。
问最后的字符矩阵第$x$行第$y$列的字符是什么

## 解题思路
数据范围不大，模拟操作

## 参考程序
```cpp
void solve()
{
    ll n,m,x,y;cin >> n >> m >> x >> y;
    x--;y--;
    vector<string> mp(n);
    FORLL(i,0,n-1) cin >> mp[i];
    ll p,q;cin >> p >> q;
    vector<pll> op(q);
    FORLL(i,0,q-1) cin >> op[i].first >> op[i].second;
    char tc;
    while(p--){
        FORLL(i,0,q-1){
            if(op[i].first==1){//行向右循环移动一次
                tc=mp[op[i].second-1][m-1];
                FORLL_rev(j,m-1,1) mp[op[i].second-1][j]=mp[op[i].second-1][j-1];
                mp[op[i].second-1][0]=tc;
            }else{//列向下循环移动一次
                tc=mp[n-1][op[i].second-1];
                FORLL_rev(j,n-1,1) mp[j][op[i].second-1]=mp[j-1][op[i].second-1];
                mp[0][op[i].second-1]=tc;
            }
        }
    }cout << mp[x][y] << endl;
}
```

# D.守恒
**数学**
## 题意
给定$n$个正整数，可以将它们的和重新分配为$n$个正整数。
求所有分配方法中，这$n$个数的最大公约数有多少种可能性。

## 解题思路
先对所有数求和，记为$sum$。
假设这$n$个数的最大公约数为$g$，则$g$一定是$sum$的因子。
且因为这$n$个数至少都为$g$，所以$sum/g\ge n$。

$O(\sqrt(n))$遍历$sum$的因子，判断是否满足条件

注意特判：当$n=1$时，答案为$1$

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    ll sum=0,t;
    FORLL(i,1,n) {cin >> t;sum+=t;}
    if(n==1) {cout << 1 << endl;return;}//特判
    ll ub=sqrt(sum)+1;
    ll ans=0;
    FORLL(i,1,ub){
        if(i*i>sum) break;
        if(sum%i==0){
            t=sum/i;
            if(t==i){
                if(t>=n) ans++;
            }else{
                if(t>=n) ans++;
                if(i>=n) ans++;
            }
        }
    }cout << ans << endl;
}
```

# E.漂亮数组
**思维**
## 题意
给定一个长度为$n$的整数数组$a$和一个正整数$k$。
可以将$a$划分成任意个非空子串，如果子串和能被$k$整除，则称这个子串是漂亮的。
求 使得漂亮子串最多的方案 得到的漂亮子串的个数。

## 解题思路
从前往后遍历，计算（从上个割点开始的）前缀和对$k$的余数$pre$。
如果当前位置的前缀和余数$pre_i$在上个割点之后的位置$j$有出现过，即$pre_i=pre_j$，则子串$[j+1,i]$区间和能被$k$整除。

因此，我们可以记录每个前缀和对$k$的余数在上个割点之后是否出现过，贪心的求出最多的漂亮子串个数。

时间复杂度：$O(n)$

## 参考程序
```cpp
void solve()
{
    ll n,k,t;cin >> n >> k;
    map<ll,int> mp;
    mp[0]=1;//余数为0即空
    ll sum=0,ans=0;
    FORLL(i,1,n){
        cin >> t;
        sum+=t;
        if(mp[sum%k]){//如果当前 前缀和对k的余数在上个割点之后出现过
            ans++;
            sum=0;
            mp.clear();//清空map
            mp[0]=1;
        }else{
            mp[sum%k]=1;
        }
    }cout << ans << endl;
}
```

# G.数三角形（easy）
**计数**
## 题意
将`*`看作实体，`.`看作空白，等腰三角形具有类似以下的形状且不可旋转：
```
一阶：
.*.
*** 

二阶：
..*..
.*.*.
*****

三阶：
...*...
..*.*..
.*...*.
*******
```

以此类推，给定一个$n\times m$的字符矩阵，求有多少个等腰三角形

## 解题思路
枚举三角形的顶点，然后向两边延展，判断是否满足条件。

对于底边，如果每次都从左往右扫描，则时间复杂度较高。
可以预处理每一个`*`在该行连续最左的`*`的位置，使用时直接查询$O(1)$判断。

## 参考程序
```cpp
void solve()
{
    ll n,m;
    char mp[N][N]={0};//字符矩阵
    cin >> n >> m;
    FORLL(i,1,n) FORLL(j,1,m) cin >> mp[i][j];
    ll pre[N][N]={0};
    FORLL(i,1,n){//预处理
        FORLL(j,1,m){
            if(mp[i][j]=='*'){
                if(pre[i][j-1]) pre[i][j]=pre[i][j-1];//如果当前位置是*，且左边有*，则pre[i][j]等于左边的*的位置
                else pre[i][j]=j;//否则pre[i][j]等于当前位置
            }
            else pre[i][j]=0;
        }
    }
    // FORLL(i,1,n) FORLL(j,1,m) cout << pre[i][j] << " \n"[j==m];//test
    ll ans=0,t;
    FORLL(i,1,n) FORLL(j,1,m) if(mp[i][j]=='*'){
        ll l=j-1,r=j+1,d=i+1;
        //d表示当前底边所在的行，l和r表示底边的两个端点位置
        while(l>=1&&r<=m&&d<=n&&mp[d][l]=='*'&&mp[d][r]=='*'){
            t=0;
            if(pre[d][r]<=l) t=1;
            if(t) ans++;
            l--;r++;d++;
        }
    }cout << ans << endl;
}
```