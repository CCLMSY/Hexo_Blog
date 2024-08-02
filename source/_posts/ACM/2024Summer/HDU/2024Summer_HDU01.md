---
title: 题解|2024暑期杭电多校01
date: 2024/7/19
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU01
description: 题解|2024暑期杭电多校01
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（1）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A81%A3%A9&source=1&searchmode=source)

# (1001)HDU7433.循环位移
**字符串**
## 题意
给定两个字符串 $A,B$ 。
定义 $[A]$ 为字符串 $A$ 的循环位移任意次可以得到的所有字符串的集合。
求 $B$ 包含 $[A]$ 中元素的个数。

## 解题思路
利用字符串Hash快速匹配。
将 $[A]$ 中所有元素的Hash记录到一个set：计算 $A+A$ 的Hash前缀和，以快速得到所有长度为 $|A|$ 的子串的Hash值，并加入set中。
枚举 $B$ 的所有长度为 $|A|$ 的子串，计算Hash值，判断是否在set中，计数。

## 参考程序
```cpp
void solve()
{
    string a,b;
    cin >> a >> b;
    ll n=a.length(),m=b.length();
    set<pll> st;
    strHash sa(a+a);
    FORLL(i,1,n)
        st.insert(sa.findz(i,i+n-1));
    strHash sb(b);
    ll ans = 0 ;
    FORLL(i,1,m-n+1)
        if(st.count(sb.findz(i,i+n-1))) ans++;
    cout << ans << endl;
}
```

# (1002)HDU7434.星星
**背包DP**
## 题意
小A要进行n次选择，每次可以选择一项：
1. 不执行操作
2. 付出$a_i$点代价得到1颗星星
3. 付出$b_i$点代价得到2颗星星
4. 付出$c_i$点代价得到3颗星星
5. 付出$d_i$点代价得到4颗星星

求恰好得到$k$颗星星的最小代价。

## 解题思路
一眼顶针鉴定为背包DP的分组背包问题。

$dp_x$表示选x个物品的最低cost。
在第$i$组时，从大到小遍历作出选择后有$j$个星星。
遍历这一步的选择（ $l$ 个星星），从 $dp_{j-l}$ 更新到 $dp_j$ ，这样更新保证了一次操作只生效一项。

## 参考程序
```cpp
void solve()
{
    ll n,k;cin >> n >> k;
    vector<array<ll,5>> cost(n,{0,0,0,0,0});
    vector<ll> dp(k+1,INF); dp[0]=0; //选i个物品的最低cost
    FORLL(i,0,n-1) cin >> cost[i][1] >> cost[i][2] >> cost[i][3] >> cost[i][4];
    FORLL(i,0,n-1){ //第i步
        FORLL_rev(j,k,1){ //选择后的个数
            FORLL(l,0,4){ //这一步选择l个
                if(j-l>=0) chmin(dp[j],dp[j-l]+cost[i][l]);
            }
        }
    }
    cout << dp[k] << endl;
}
```

# (1008)HDU7440.位运算
## 题意
给定整数$n,k$，求满足 $((a\otimes b)\oplus c)\ominus d=n$ 的四元组 $(a,b,c,d)，0\le a,b,c,d\lt 2^k$ 的个数。

其中，$\otimes$ 表示按位与，$\oplus$ 表示按位异或，$\ominus$ 表示按位或。

## 解题思路
转为二进制，按位考虑。

若 $n$ 的某一位上是 $0$ ， $d$ 在这一位上必须为 $0$ ，$c$ 在这一位上由 $a,b$ 决定（控制这一位为 $0$），$a,b$ 在这一位上任选，即 $1\times 1\times 2\times 2=4$ 种可能。

若 $n$ 的某一位上是 $1$ ：
1. 若 $d$ 在这一位上为 $0$ ，则 $c$ 在这一位上由 $a,b$ 决定（控制这一位为 $1$），$a,b$ 在这一位上任选，即 $1\times 1\times 2\times 2=4$ 种可能。
2. 若 $d$ 在这一位上为 $1$ ，则 $a,b,c$ 在这一位上任选，即 $1\times 2\times 2\times 2=8$ 种可能。

综上，$n$ 的一位上是 $1$ 时有 $12$ 种可能，是 $0$ 时有 $4$ 种可能。

答案为 $4^k\times 3^{cnt1}$ 。

## 参考程序
```cpp
void solve()
{
    ll n,k;
    cin >> n >> k;
    ll cnt1=0;
    while(n){
        if(n%2) cnt1++;
        n/=2;
    }
    cout << qcpow(2,k*2)*qcpow(3,cnt1) << endl;
}
```

# (1012)HDU7444.并
## 题意
给定 $n$ 个矩形，对 $k=1\to n$ 分别求随机取 $k$ 个矩形的面积并的期望。

## 解题思路
平面可以被矩形边界分割成若干个小区域，考虑每个区域对答案的贡献。

因为要求期望，被“相同数量矩形覆盖”的小区域 对答案的贡献是相同的，因此按照 覆盖矩形数量 将这些小区域分组，统计出恰好被 $i$ 个矩形覆盖的区域的面积 $S_i$。

我的求法就是从左到右扫描，在每条竖线的位置，更新 $i$ 个矩形覆盖的y轴长度 $yval_i$ ，到下一条竖线再乘经过的x轴长度，得到这一部分的面积（类似于积分？），加入 $S_i$ 

然后考虑贡献的权重。
从 $n$ 个矩形中随机选取 $k$ 个，对于被 $i$ 个矩形覆盖的区域，只要选取的 $k$ 个矩形中，存在这 $i$ 个矩形之一，那么这个面积就会被计入。
在上述条件下的贡献权重是 $val_{k,i}=\frac{C_{n}^{k}-C_{n-i}^{k}}{C_{n}^{k}}$ ，即从 $n$ 个矩形中选取 $k$ 个方案数，减去从 $n-i$ 个矩形中选取 $k$ 个方案数。

最后答案$ans_k$是 $\sum\limits_{i=1}^{n}val_{k,i}\times S_i$ 。

## 参考程序
```cpp
void prepare(){
    Prepare_Combination(5005);
    MOD = 998244353;
}
ll getval(ll n,ll i,ll j){
    ll ret=C[n][i],sb;
    if(i>n-j) sb=0;
    else sb = C[n-j][i];
    return sub(ret,sb);
}
void solve()
{
    ll n;cin >> n;
    ll xx1,xx2,yy1,yy2;
    vector<tuple<ll,ll,ll,ll>> xlines;
    vector<ll> ylines;
    // xline:(y,x1,x2,flag) # 横线 flag:0上边界1下边界
    // yline:(x) # 竖线x坐标
    FORLL(i,1,n)
    {
        cin >> xx1 >> yy1 >> xx2 >> yy2;
        xlines.emplace_back(yy1,xx1,xx2,0);
        xlines.emplace_back(yy2,xx1,xx2,1);
        ylines.emplace_back(xx1);
        ylines.emplace_back(xx2);
    }
    SORT(xlines); SORT(ylines);

    map<ll,ll> S,yval;
    ll prex=0;
    // S[i]:重叠i次部分的面积
    for(auto xx:ylines)
    {
        ll difx = xx-prex;
        for(auto [pl,val]:yval)
        {
            // S[pl]+=difx*val;
            addto(S[pl],mul(difx,val));
        }
        yval.clear();
        ll cur = 0, prey = 0;
        for(auto [yy,xx1,xx2,flag]:xlines)
        {
            if(xx1<=xx&&xx<xx2)
            {
                yval[cur]+=yy-prey;
                if(flag==0) cur++;
                else cur--;
                prey=yy;
            }
        }
        prex = xx;
    }
    // for(auto x:S) cout << x.first << ' ' << x.second << endl;

    FORLL(i,1,n){
        ll ans = 0;
        FORLL(j,1,n){
            ll val = getval(n,i,j);
            ans = add(ans,mul(val,S[j]));
        }
        divto(ans,C[n][i]);
        cout << ans << endl;
    }
}
```