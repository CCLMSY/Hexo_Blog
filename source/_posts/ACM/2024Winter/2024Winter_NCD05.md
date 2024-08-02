---
title: 题解|2024牛客寒假05
tags:
  - 2024牛客寒假
categories: 算法竞赛
mathjax: true
abbrlink: ACM_2024Winter_NCD05
date: 2024/2/21
description: 题解|2024牛客寒假05
---

# A.mutsumi的质数合数
**1签**
## 题意
给定一个正整数集合，问其中质数和合数的个数之和。

## 解题思路
正整数中只有$1$既不是质数也不是合数，总数减去$1$的个数即为答案

(P.S.)：有人这题WA了4发。。。

## 参考程序
```cpp
void solve()
{
    ll n,t,cnt=0;
    cin >> n;
    FORLL(i,1,n){
        cin >> t;
        if(t==1) cnt++;
    }cout << n-cnt << endl;
}
```

# C.anon的私货
**贪心**
## 题意
给定一个长度为$n$的正整数序列，可以在其中任意位置插入$0$。
在保证除全$0$外的任意子序列的平均值大于$1$的条件下最多可以插入多少个$0$。

## 解题思路
题目可以转换为：对于每个数字，两边插入的$0$的个数之和不大于它。

贪心的，先从左往右扫描，对于每个数字，尽量往左插入$0$，
第$i$个数字$a_i$左边插入$0$的个数记为$pl_i$。

最左边的数字$a_1$可以往左插入$a_1-1$个$0$，
后面的数字$a_i$允许往左插入$0$的个数$pl_i$被限制为以下两个值的最小值：
1. $a_i-1$：$a_i$左边插入的数量最多为$a_i-1$
2. $a_{i-1}-1-pl_{i-1}$：$a_{i-1}$两边插入的数量$pl_{i-1}+pl_i<=a_{i-1}-1$

接着，从右往左扫描，对于每个数字，尽量往右插入$0$，类比上面的过程。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    create_vec(v,n);
    vector<ll> pl(n+1,0); //pl[i]表示第i个位置左边的空当，pl[n]表示最右端
    pl[0]=v[0]-1;
    pl[n]=v[n-1]-1;
    FORLL(i,1,n-1){
        pl[i]=min(v[i-1]-1-pl[i-1],v[i]-1);
    }
    FORLL_rev(i,n-1,1){
        pl[i]=min({v[i]-1-pl[i+1],v[i-1]-1,pl[i]});
    }
    ll ans=0;
    FORLL(i,0,n) ans+=pl[i];
    if(n==1) ans=v[0]-1;//特判
    cout << ans << endl;
}
```

# E.soyorin的数组操作（easy）
**贪心**
## 题意
给定一个长度为$n$的数组，每次可以选定一个$n$以内的偶数$k$，使得$a_1$到$a_k$这$k$个数的值都增加它们的下标。
问任意次操作后能否使得数组不严格单调递增。

## 解题思路
如果$n$为偶数，每次选中全数组都可以使相邻两个数之差（后一个减前一个）增加$1$，显然必定可以使得数组递增。

如果$n$为奇数。
从后往前遍历，对于每个偶数位置$i$，在满足$a_i<=a_{i+1}$的前提下使其最大。
这样做可以保证，在合法的情况下，这个数减前面的数最大。

同时记录操作次数，同步到前面的元素。

最后判断数组是否有序。

## 参考程序
```cpp
void solve()
{
    ll n,t;cin >> n;
    create_vec(v,n);
    if(n%2==0){ cout << "YES" << endl; return; }//偶数个数，直接YES

    ll cur=0;
    FORLL_rev(i,n-2,0){
        v[i]+=cur*(i+1);
        v[i-1]+=cur*i;//同步更新左边的值
        if(v[i]>v[i+1]){ cout << "NO" << endl; return; }
        //v[i+1]已经是满足条件下最大的值了。如果还是小于v[i]，那么就不可能满足条件了
        t=(v[i+1]-v[i])/(i+1);
        v[i]+=t*(i+1);
        v[i-1]+=t*i;
        cur+=t; i--;
    }
    
    FORLL(i,0,n-2) if(v[i]>v[i+1]){ cout << "NO" << endl; return; }
    cout << "YES" << endl;
}
```

# GH.sakiko的排列构造（easy+hard）
**构造**
## 题意
给定一个正整数$n$，构造一个$n$的排列$p$，使得$p_i+i$全为质数

## 解题思路
考虑一段连续的数字$[l,r]$，如果两端之和$l+r$是质数，那么把这段数字倒过来，与原来的数字序列对应相加，将得到$r-l+1$个质数$r+l$。

先确定右端点$r=n$，从后往前寻找一个左端点$l$，使得$l+r$为质数，再将这个区间的数字序列倒过来放入这个区间。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    vector<ll> ans(n+1,0);
    Phi_and_Prime_Table(2*n+2);//预处理质数表，数据区间有限暴力判断也可
    ll r=n;
    FORLL_rev(i,n,1){
        if(!check[i+r]){//check[i+r]==0表示i+r是质数
            FORLL(j,i,r) ans[j]=r+i-j;
            r=i-1;
        }
    }
    FORLL(i,1,n) cout << ans[i] << Presentation(i,n);
}
```

# I.rikki的最短路
**思维**
## 题意
在一条数轴上，你在原点$O$，$A$在$a$处，$T$在$t$处，你的视线区间为$k$。
你先去找$T$。

如果路上看到$A$，先去找$A$，再去找$T$。
如果路上没看到$A$，先去去找$T$，再去找$A$，再去找$T$。

问最短路程。

## 解题思路
分类讨论

1. $t>=0$：$T$在原点右边，去找$T$可以看到的区间为$[-k,t+k]$
   1. $A$在$[-k,0]$：先去找$A$，再去找$T$，最短路程为$t+2|a|=t-2a$
   2. $A$在$[0,t]$：路上把$A$捡走，再去找$T$，最短路程为$t$
   3. $A$在$[t,t+k]$：先去找$T$，再去找$A$，再去找$T$，合并到其他情况
   4. 其他：先去找$T$，再去找$A$，再去找$T$，最短路程为$t+2|a-t|$
2. $t<0$：$T$在原点左边，类比即可

## 参考程序
```cpp
void solve()
{
    ll t,a,k;cin >> t >> a >> k;
    if(t>=0){
        if(a>=-k&&a<=t){
            if(a<=0) cout << t-a*2 << endl;
            else cout << t << endl;
            return ;
        }else{
            cout << t+abs(t-a)*2 << endl;
            return ;
        }
    }else{
        if(a>=t&&a<=k){
            if(a>=0) cout << a*2-t << endl;
            else cout << -t << endl;
            return ;
        }else{
            cout << -t+abs(t-a)*2 << endl;
            return ;
        }
    }
}
```

# J.rikki的数组陡峭值
**贪心**
## 题意
长度为$n$的数组，每个元素$a_i$限定在一个区间$[l_i,r_i]$内。

定义陡峭值为每对相邻元素的差的绝对值之和$\sum\limits_{i=1}^{n-1}|a_{i+1}-a_i|$，求陡峭值的最小值。

## 解题思路
从前往后遍历。

如果$[l_i,r_i]$和当前区间有交集，那么都取交集内的数字，陡峭值为$0$。
如果$[l_i,r_i]$和当前区间没有交集，那么取两个区间的边界，将当前区间退化成一点。
退化后，改为用前一点和区间作比。在区间外，取区间边界。

## 参考程序
```cpp
void solve()
{
    ll l=0,r=1e9+5,pre=0,ans=0;
    int flag=1;
    ll n;cin >> n;
    ll tl,tr;
    FORLL(i,1,n){
        cin >> tl >> tr;
        if(flag){//当前为一区间
            if(tr<=l||tl>=r){//元素a_i区间与当前区间不相交
                flag=0;
                if(tr<=l){
                    ans+=abs(l-tr);
                    pre=tr;
                }else{
                    ans+=abs(tl-r);
                    pre=tl;
                }//区间退化
            }else{
                if(tl>=l) l=tl;
                if(tr<=r) r=tr;
            }
        }else{//当前为一点
            if(tl<=pre&&tr>=pre) continue;
            if(tl>=pre){
                ans+=abs(tl-pre);
                pre=tl;
            }
            if(tr<=pre){
                ans+=abs(pre-tr);
                pre=tr;
            }
        }
    }cout << ans << endl;
}
```

# K.soyorin的通知
**完全背包**
## 题意
你要通知$n$个人。你通知1个人花费$p$，第$i$个人通知$a_i$个人花费$b_i$。问最少花费。

## 解题思路
首先你至少需要通知$1$个人，才能使他们相互传递通知。
再让这个人去通知其他人，以此传递，直到其他$n-1$个人都被通知到。
把$a_i$看作容量，$b_i$看作价值，每个人可以选多次，问题转化为求最小价值完全背包问题。
DP前，也要把自己作为$a=1,b=q$的物品加入，因为自己也可以单独通知多个人。

## 参考程序
```cpp
void solve()
{
    ll n,p,t;cin >> n >> p;
    vector<ll> cost(n+1),val(n+1);
    FORLL(i,1,n) cin >> cost[i] >> val[i];
    cost[0]=p;val[0]=1;
    vector<ll> dp(n+1,INF);
    dp[0]=0;
    FORLL(i,0,n)
        FORLL(j,0,n-2){
            t=j+val[i]>n-1?n-1:j+val[i];
            dp[t]=min(dp[t],dp[j]+cost[i]);
        }
    cout << dp[n-1]+p << endl;
}
```

# L.rikki的星星
**2签**
## 题意
进行$n$局游戏，赢一局$+1$分，输一局$-1$分，没有平局。

游戏结束后分数为$x$，问赢多少局输多少局，或是无解。

## 解题思路
赢一局和输一局的分差为$2$，因此最终得分和局数的奇偶性一定相同。

记赢了$a$局，则：$a+(n-a)=x$，解得$a=\dfrac{x+n}{2}$。

## 参考程序
```cpp
void solve()
{
    ll n,x;cin >> n >> x;
    if((x+n)%2){
        cout << "-1" << endl;
        return;
    }
    cout << (x+n)/2 << ' ' << (n-x)/2 << endl;
}
```

# M.rikki的排列连通
**贪心**
## 题意
给定$2$个$n$的排列，置入$2\times n$的矩阵。
每次操作可以消掉同一数字所在的方块。

问最少操作多少次可以把矩阵分为没有相邻边的两部分（及以上）。

## 解题思路
以下是几种合法的情况：
1. 某个数字在两个排列中的位置相同，且两边都有数字，消除它即可，答案为$1$。
2. 某个数字在两个排列中的位置相差$1$，交叉的部分一定有数字，消除它即可，答案为$1$。
3. 每个数字在两个排列中的位置都至少相差$2$，选择同一位置的两个数字消除，答案为$2$。

特判是`n=1,{1},{1}`、`n=2,{1,2},{1,2}`、`n=2,{2,1},{2,1}`。
第一种情况中，某个数字在两个排列中都位于最左或都位于最右，这个数字不能计入答案。

## 参考程序
```cpp
void solve()
{
    ll n,t; cin >> n;
    ll mndif=INF;//最小差值
    vector<ll> pos(n+1);
    pos.resize(n+1);
    FORLL(i,1,n){
        cin >> t;
        pos[t]=i;
    }
    FORLL(i,1,n){
        cin >> t;
        if(i==1&&pos[t]==1) ;//最左
        else if(i==n&&pos[t]==n) ;//最右
        else mndif=min(mndif,abs(i-pos[t]));
    }
    if(n==1) {cout << "-1" << endl;return ;}
    if(n==2&&mndif>1) {cout << "-1" << endl;return ;}//特判
    if(mndif<2) {cout << "1" << endl;return ;}
    cout << "2" << endl;
}
```

