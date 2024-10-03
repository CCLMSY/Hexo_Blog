---
title: 题解|2024暑期牛客多校06
date: 2024/8/1
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD06
description: 题解|2024暑期牛客多校06
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

比赛链接：[2024牛客暑期多校训练营6](https://ac.nowcoder.com/acm/contest/81601)

# A.Cake
## 题意
给定一棵 $n$ 个节点，以 $1$ 为根的树，每条边有一个权值 0 或 1。
棋子初始在节点 $1$ ，Alice先手，Bob后手。
每回合，玩家可以将棋子移动到当前节点的一个子节点。
直到棋子达到叶子节点，游戏结束。

棋子经过的路径的权值按顺序构成一个01序列，记它的长度为 $m$ 。
接下来，Bob可以把 $1$ 块蛋糕任意分成 $m$ 份（某份蛋糕可以为0//空盘子）。
然后根据01序列决定取蛋糕的次序：0代表Bob取，1代表Alice取。

问两人都以最优策略行动和切蛋糕，问Alice最多能拿到多少蛋糕。

## 解题思路
先考虑在路径确定的情况下，Bob会怎么分蛋糕：
Bob会找到一个前缀pre，这个前缀的“0占比”是所有前缀中最大的。
pre的长度记为 $t$，0占比记为 $p$。
Bob会将蛋糕平均分为 $t$ 份，剩下 $m-t$ 份为空。
这样，Bob可以保证他拿到最多的，大小为 $p$ 的蛋糕。

对于每个节点 $i$ ，先DFS一次，处理出从 $1$ 到 $i$ 这条路径上所有前缀的最大0占比。
那么在每个结点处。

再DFS一次，假设根节点 $1$ 的深度是1，那么：
深度为奇数的节点是Alice的回合，Alice会希望前缀的“0的占比”最小化；
深度为偶数的节点是Bob的回合，Bob会希望前缀的“0的占比”最大化。

从叶子到根更新，得到最终0占比 $p$ ，按照上面的方法分蛋糕，最终答案为 $1-p$ 。

## 参考代码
```cpp
ll n;
vector<vector<pll>> G;
vector<ld> pref; //前缀中0的最大占比
void dfs(ll u=1,ll fa=0,ll cnt0=0,ll cnt1=0){
    if(u!=1) chmax(pref[u],(ld)cnt0/(cnt0+cnt1));//用当前节点的0占比更新前缀
    for(auto [v,w]:G[u])
        if(v!=fa){
            pref[v]=pref[u];//继承父节点的pref
            dfs(v,u,cnt0+(w==0),cnt1+(w==1));
        }
}
vector<ld> dp; //节点对执棋手的最优解
void dfs_dp(ll u=1,ll fa=0, ll dep=1){
    dp[u]=dep%2; 
    if(u!=1&&G[u].size()==1) dp[u]=pref[u];
    for(auto [v,w]:G[u]) if(v!=fa){
        dfs_dp(v,u,dep+1);
        if(dep%2) chmin(dp[u],dp[v]); //先手求pref小
        else chmax(dp[u],dp[v]); //后手求pref大
    }
}
void solve()
{
    cin >> n;
    G.clear(); G.resize(n+1);
    pref.clear(); pref.resize(n+1,0);
    dp.clear(); dp.resize(n+1,0);
    ll u,v,w;
    FORLL(i,1,n-1){
        cin >> u >> v >> w;
        G[u].emplace_back(v,w);
        G[v].emplace_back(u,w);
    }
    dfs(); dfs_dp();
    print_float(1.-dp[1],12);
    cout << endl;
}
```

# B.Cake2
## 题意
给定正整数 $n,k$ ，蛋糕的形状是正 $n$ 边形，顶点编号为 $0$ 到 $n-1$ 。
对于每个顶点 $i$ ，沿着由 顶点 $i$ 和 $(i+k)mod\ n$ 确定的直线切一刀。
求最后蛋糕的块数。

以$n=6,k=2$为例，蛋糕的形状如下：
![](/images/ACM/2024Summer_NCD06_B.png)

## 解题思路
手画几个图，发现当且仅当 $n=k\times2$ 时，蛋糕的块数为 $n$ 。
其余情况下，发现每个交点仅由2条直线，蛋糕的块数为 $n\times k+1$ 。

## 参考代码
```cpp
void solve()
{
    ll n,k;cin >> n >> k;
    if(k>n/2) k=n-k;
    if(k*2!=n) cout << k*n+1 << endl;
    else cout << n << endl;
}
```

# H.Genshin Impact's Fault
## 题意
给定一个字符串，包含以下字符：
- `3`：三星卡
- `4`：四星卡
- `5`：普通五星卡
- `U`：特殊五星卡

满足以下条件的字符串是合法的：
1. 每10个连续的字符不能都是`3`
2. 每90个连续的字符至少有1个`U`或`5`
3. 相邻两张五星卡不能都是`5`

判断给定字符串是否合法。

## 解题思路
按照题意模拟判断。

## 参考代码
```cpp
string s;
bool check1(){
    ll cnt=0;
    for(auto c:s){
        if(c=='3') cnt++;
        else cnt=0;
        if(cnt>=10) return false;
    }return true;
}
bool check2(){
    ll cnt=0;
    for(auto c:s){
        if(c!='5'&&c!='U') cnt++;
        else cnt=0;
        if(cnt>=90) return false;
    }return true;
}
bool check3(){
    char pre='0';
    for(auto c:s){
        if(c=='5'||c=='U'){
            if(pre=='0') pre=c;
            else if(pre=='5'&&c=='5') return false;
            else pre=c;
        }
    }return true;
}
void solve()
{
    cin >> s;
    if(check1()&&check2()&&check3()) cout << "valid\n";
    else cout << "invalid\n";
}
```