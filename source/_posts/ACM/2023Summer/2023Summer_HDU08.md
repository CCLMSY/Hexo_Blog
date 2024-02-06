---
title: 题解|2023暑期杭电多校08
date: 2023/8/10
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: a4f57cd6
description: 题解|2023暑期杭电多校08
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

# 1005.0 vs 1
**博弈，模拟**
## 题目大意
两人名为零和壹，在给定的 $01$ 串上进行博弈
零只能取走两端的某一个 $0$ ，壹只能取走两端的某一个 $1$ ，零执先
先不能取的人判负，若取完则判平局

## 解题思路
模拟博弈过程，当前操作者 $x$ 可以可以遵循以下策略：
1. 两端不同，只能取 $x$ 的一端，交替操作权
2. 两端相同
   1. 两端都不是 $x$ ，无法操作，失败
   2. 两端都是 $x$ ，假设取了某端
      1. 这端的下一个数字是 $x$ ，则两端都是 $x$ ，对方无法操作，获胜
      2. 这端的下一个数字是 $!x$ ，则对方只能取这一端
   3. 如果离任一端最近的连续两个相同的数都为 $x$ ，则根据上 $2$ 一直取到 $x$ 获胜
   4. 如果离两端最近的连续两个相同的数都为 $!x$ ，则不论选哪端，最终都会到达两端都为 $!x$ 的情况，失败
      1. 特判：如果整个串有且仅有 $1$ 段连续两个相同的 $!x$ ，则从两端向中间将各会取掉一个，达成平局 

可以结合代码注释理解这一过程
### 时间复杂度
$O(n)$

## 参考代码
```cpp
void solve_s(deque<int> s,int now){
    ll n=s.size(),i,j;;
    if(s.empty()) {cout << "-1" << endl;return ;}//取完平局
    if(s.front()!=s.back()){//前后不同，无法自由选择，交替操作权
        if(s.front()==now){//取前
            s.pop_front();solve_s(s,(now?0:1));return ;
        }else{//取后
            s.pop_back(); solve_s(s,(now?0:1));return ;
        }
    }else{//前后相同
        if(s.front()!=now) {cout << (now?"0":"1") << endl;return ;}//两端不可取，失败
        for(i=0;i<n-1;i++) if(s[i]==s[i+1]) break;//从前往后找第一个连续
        if(i==n-1) {cout << "-1" << endl;return ;}//无连续，取完平局
        if(s[i]==now) {cout << (now?"1":"0") << endl;return ;}//有己方连续，胜利
        for(j=n-1;j>i;j--) if(s[j]==s[j-1]) break;//从后往前找最后一个连续
        if(s[j]==now) {cout << (now?"1":"0") << endl;return ;}//有己方连续，胜利
        if(i+1==j) {cout << "-1" << endl;return ;}//有且仅有一对对方连续，取完平局
        {cout << (now?"0":"1") << endl;return ;}//两对以上对方连续，失去主动权，判负
    }
}
void solve(){
    ll n;cin >> n;
    string ts;cin >> ts;
    deque<int> s(n);FORLL(i,0,n-1) s[i]=ts[i]-'0';
    solve_s(s,0);
}
```

***

# 1007.Solubility
**并查集/DFS**
## 题目大意
给定 $n$ 个元素之间的 $m$ 对等价关系，问指定 $k$ 个元素是否属于同一等价类

## 解题思路
这里给出两种解题思路：
1. DFS：建无向图，DFS判断指定元素是否在同一个连通分量里
2. 并查集：标准并查集板子题，裸套即可

## 参考代码
1. DFS
```cpp
#define N 100005
int visited[N]={0};
vector<vector<ll>> G;
void DFS(ll x){
    visited[x]=1;
    for(auto v:G[x]){
        if(!visited[v]) DFS(v);
    }
}
void solve()
{
    ll m,n;cin >> n >> m;
    G.clear();G.resize(n+1);
    FORLL(i,1,n) visited[i]=0;
    ll u,v;
    FORLL(i,1,m){
        cin >> u >> v;
        G[u].emplace_back(v);
        G[v].emplace_back(u);
    }
    ll k;cin >> k;
    vector<ll> s(k);for(auto &x:s) cin >> x;
    DFS(s[0]);
    for(auto x:s) if(!visited[x]) {cout << NO;return ;}
    cout << YES;
}
```

2. 并查集
```cpp
void solve(){
    ll n,m;cin >> n >> m;
    DSU dsu(n);
    ll a,b;
    FORLL(i,1,m){
        cin >> a >> b;
        dsu.merge(a,b);
    }
    ll k;cin >> k >> a;a=dsu.find(a);
    FORLL(i,2,k){
        cin >> b;b=dsu.find(b);
        if(b!=a) {cout << NO;FORLL(j,i+1,k) cin >> n;return ;}
    }cout << YES;
}
```

***

# 1008.Expectation of Rank
**线性代数-矩阵与向量空间、期望、动态规划**
## 题目大意
给定两个正整数 $n,p$ ，其中 $p$ 是质数
 $n$ 阶矩阵 $\bf A$ 中的所有元素随机在 $p$ 的有限域 $\mathbb{F}_p$ 中产生，求矩阵 $\bf A$ 的秩的期望 $\mathbb{E}(rank(\bf A))$ ，答案取模

### 前置知识点
1. [有限域](https://baike.baidu.com/item/%E6%9C%89%E9%99%90%E5%9F%9F/4273049) $\mathbb{F}_p$ ：在本题中可以粗略的理解为 $[0,p-1]$ 的整数集
2. 线性代数-矩阵与向量空间基础知识：多维向量与向量组、线性相关、矩阵的秩与向量之间的关系、向量组张成向量空间的概念等

建议在学习过《线性代数》课程后再解决本题。

## 解题思路
一个含 $k$ 个向量的极大无关组可以张成一个 $k$ 维向量空间

在 $p$ 的有限域 $\mathbb{F}_p$ 下，每一维度上的坐标有 $p$ 种选择，故以该极大无关组为基，通过线性组合可以产生 $p^k$ 种不同的 $k$ 维向量（高维包含低维）
顺带一提，这并不意味着这 $p^k$ 种向量仅有 $k$ 位坐标非 $0$ 

矩阵 $\bf A$ 的每一行可以视为一个 $n$ 维向量，前 $i$ 行的秩表示了前 $i$ 个向量组成的向量组，其极大无关组中有多少个向量。这也意味着前 $i$ 行已经张成了一个多少维度的向量空间

构造DP数组， $dp_{i,k}$ 用以表示矩阵 $\bf A$ 前 $i$ 行的秩为 $k$ 的方案数

假设前 $i-1$ 行的秩为 $k$ ，那么其张成的向量空间为 $k$ 维，考虑状态转移：
1. 第 $i$ 行中可以构造出 $p^k$ 个向量落在这个向量空间中，并不改变秩（或者说维数）
2. 余下 $p^n-p^k$ 个向量将与前 $i-1$ 个向量线性无关，并使张成的空间增大一维，秩 $+1$

综上所述，构造出以下状态转移方程：
$$dp_{i,k}=
\begin{cases}
    0 \qquad,k>i\quad(rank_i\le i恒成立) \newline
    1 \qquad,k=0\quad(当且仅当全为 \bf{0} \text{向量}) \newline
    \sum\limits_{j=1}^{i-1} dp_{i-1,j}\times p^j + dp_{i-1,j-1}\times (p^n-p^j) &,Otherwise
\end{cases}
$$

总方案数为 $(p^n)^n=p^{n^2}$ ，最终期望为 $\dfrac{1}{p^{n^2}}\sum\limits_{j=1}^{n} j\times dp_{n,j}$

### 时间复杂度
DP：$O(n^2)$

## 参考代码
```cpp
#define N 5005
ll n,p;
ll dp[N][N],powp[N];//dp_i,k表示矩阵前i行秩为k的方案数
void Init(ll n){
    FORLL(i,0,n)
        FORLL(j,0,n)
            dp[i][j]=0;
    powp[0]=1;
    dp[0][0]=1;
    FORLL(i,1,n) powp[i]=mul(powp[i-1],p);
}
void solve()
{
    cin >> n >> p;
    Init(n);
    FORLL(i,1,n){
        dp[i][0]=1;
        FORLL(k,1,i){//每行看作一个向量，有p^n种构造法。确定前i行的秩为k的总向量方案数
            addto(dp[i][k],mul(dp[i-1][k],powp[k]));
            //对于前i-1行秩为k的某方案，其向量空间为k维，故在p的有限集下有且仅有p^k个向量落在该向量空间
            addto(dp[i][k],mul(dp[i-1][k-1],sub(powp[n],powp[k-1])));
            //对于前i-1行秩为k-1的某方案，有p^(k-1)个向量落在其张成的向量空间，剩余向量与前i-1行线性无关
        }
    }
    ll re=0,ninv=inv(qcpow(powp[n],n));//ninv:总方案数(p^n)^n的倒数
    FORLL(i,1,n) addto(re,mul(mul(i,dp[n][i]),ninv));
    cout << re << endl;
}
```

***

# 1010.Rikka with Square Numbers
**数学、贪心**
## 题目大意
给定两个正整数 $a,b$ ，每次操作可以使 $a$ 增大或减小一个平方数 $x$ ，求把 $a$ 变成 $b$ 的最小操作次数 

## 解题思路
即求 $a,b$ 之差最少可以用多少个平方数的和差表示。以下是一些思路：
1. $a=b$ ， $0$
2. $n^2$ ，平方数本身， $1$
3. $n^2-(n-1)^2=2n-1$ ，用两个相邻平方数之差即可表示任意奇数
4. $n^2-(n-2)^2=4(n-1)$ ，用两个距离为 $2$ 的平方数之差可以表示任意 $4$ 的倍数
5. 结合以上两条可以归纳证明两个平方数之差一定为奇数或 $4$ 的倍数，$2$
6. 模 $4$ 余 $2$ 的情况可能为两平方数加和，可以枚举判断， $2$
7. 其余的数可以用第 $3$ 点得到任意奇数后加减 $1$ ， $3$ 

综上判定即可

### 时间复杂度
 $O(\sqrt n)$

## 参考代码
```cpp
bool Is_Sqr(ll x){
    ll t=sqrt(x);
    if(t*t==x) return 1;
    return 0;
}
void solve()
{
    ll a,b;cin >> a >> b;
    if(a<b) swap(a,b); ll dif=a-b;
    if(a==b) {cout << 0 << endl;return ;}
    if(Is_Sqr(dif)) {cout << 1 << endl;return ;}
    if(dif%2||(dif%4==0)) {cout << 2 << endl;return ;}
    for(ll i=1;i*i<=dif;i++) if(Is_Sqr(dif-i*i)) {cout << 2 << endl;return ;}
    cout << 3 << endl;
}
```