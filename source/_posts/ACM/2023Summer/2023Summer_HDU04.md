---
title: 题解|2023暑期杭电多校04
date: 2023/7/27
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 803f0ea0
description: 题解|2023暑期杭电多校04
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

# 1003.Simple Set Problem
**思维**
## 题目大意
给定 $k$ 个非空集合，在每一个集合里选择一个数 $a_i$ ，使得这些数之中最大值与最小值之差 $d=max(a_i)-min(a_i)$ 最小，输出 $d$ 的最小值
## 解题思路
不论哪一种选法，最小值一定是在全体元素中取到的
不妨从小到大遍历全体元素作为下界，记当前遍历元素为 $a_i$ 
在每一个集合中二分查找恰好大于等于 $a_i$ 的元素选入(遍历到 $a_i$ 所在集合的时候，一定会选入 $a_i$ )
找到选定元素的最大值，做差
直到某个集合中没有大于等于 $a_i$ 的元素为止（此时该集合最大的元素选入的情况已全部考虑过），最终在得到的值中取最小即可
### 时间复杂度
读入： $O(n)$ ；排序： $O(n\log n)$ ；遍历二分查找： $O(n\log n)$ 
综合时间复杂度：  $O(n\log n)$ 
其中 $n$ 表示全体元素个数， $n\le 4e6$

## 参考代码
```cpp
void solve()
{
    ll n;
    cin >> n;
    vector<vector<ll>> sets(n+1);
    vector<ll> v;
    vector<ll>::iterator it;
    ll tn,t;
    FORLL(i,1,n){
        cin >> tn;
        FORLL(j,1,tn) {cin >> t;sets[i].emplace_back(t);v.emplace_back(t);}
        SORT(sets[i]);
    }SORT(v);
    ll re=INF,tre;
    for(auto x:v){
        tre=0;
        FORLL(i,1,n){
            it=lower_bound(ALL(sets[i]),x);
            if(it==sets[i].end()) {tre=-1;break;}
            tre=max(tre,*it-x);
        }
        if(tre==-1) break;
        re=min(re,tre);
    }
    cout << re << endl;
}
```

***

# 1006.PSO
**数学题、概率论**
## 题目大意
给定一个星状图（中心节点与每个其他节点之间各有一条边，其他节点之间没有边），任选两个点，求它们之间的距离的数学期望以及最大距离的长度
## 解题思路
任选两个点的方案总数为 $C_n^2=n(n-1)/2$ 
其中一个点是中心点的方案数为 $n-1$ ，距离是 $1$
其余方案数为 $n(n-1)/2-(n-1)$ ，距离是 $2$
因此任选两个点的距离期望为：
$$\begin{align}
    E&=\{[n(n-1)/2-(n-1)]\times2+(n-1)\times1\}/C_n^2 \newline
    &=[n(n-1)-(n-1)\times2+(n-1)]/C_n^2 \newline
    &=(n-1)^2/C_n^2 \newline
    &=2(n-1)/n
\end{align}
$$
注意控制精度
## 参考程序
```cpp
void solve()
{
    ll n;
    cin >> n;
    printf("%.9lf",2.*(n-1)/n);
    if(n>2) printf(" 2.000000000\n");
    else printf(" 1.000000000\n");
}
```

***

# 1007.Guess
**莫比乌斯函数**
## 题目大意
给定一个正整数 $n\le1e18$ ，求 $e^{S(n)}$ 

其中，$S(n)=\sum\limits_{d|n} \mu(\dfrac{n}{d})\ln(d)$ ，$e$ 是自然对数 $\ln$ 的底数，$\mu(x)$ 是莫比乌斯函数，答案取模

### 前置知识点
[莫比乌斯函数](https://baike.baidu.com/item/%E9%BB%98%E6%AF%94%E4%B9%8C%E6%96%AF%E5%87%BD%E6%95%B0/16625781)：
$$
\mu(n)=\begin{cases}
    1 &,n=1; \newline
    0 &,n含有大于1的完全平方因子; \newline
    (-1)^k &,n为k个质数的乘积 \newline
\end{cases}
$$

## 解题思路
对于 $n$ 的因数 $d$ ，根据 $\dfrac{n}{d}$ 与 $d$ 的轮换对称性可知：
$S(n)=\sum\limits_{d|n} \mu(\dfrac{n}{d})\ln(d)=\sum\limits_{d|n} \mu(d)\ln(\dfrac{n}{d})$

对 $S(n)$ 做一次莫比乌斯变换（第一形式）可得：$\ln(n)=\sum\limits_{d|n} S(d)$

两边以 $e$ 做底数得 $e^{ln(n)}={e}^{\sum\limits_{d|n} S(d)}\Rightarrow n=\prod\limits_{d|n} e^{S(d)}$ 

右式是一个连乘积，其连乘长度为 $n$ 的全体因数个数
记 $F(x)=e^{S(x)}$ ，那么答案也就是在求 $F(n)$ 

根据上式：
1. $1=F(1)\Rightarrow F(1)=1$
2. 对于任意质数 $x$ ，$x=F(1)F(x)\Rightarrow F(x)=x$
3. 对于任意合数 $x$ ，借助 $1,2$ 递归计算出 $F(x)$ 

经过较长时间的感性理解，想到了一种遍历求 $F(x)$ 方法的伪代码如下：

```
F[]={0}
FOR i in {1..+∞}
    FOR p in Primes
        if F[i*p]==0 then F[i*p]=p
```
（其实打表也挺好的，想个一般性的方法好难qwq）

可以得到以下 $F(x)$ 的正整数序列：
$1,2,3,2,5,1,7,2,3,1,11,1,13,1,1,2,17,1,19,1,1,1,23\cdots$

这个序列的意义是 $x$ 对乘积 $n$ 的贡献，回到上面看那个连乘积的长度和表达形式就非常合理了

通项公式为：
$$\begin{align}
    F(n)&=lcm\{1..n\}/lcm\{1..n-1\} \newline
    &=\begin{cases}
        n&,n=p^i,p是素数,i\in \N_+ \newline
        1&,Otherwise
    \end{cases}
\end{align}
$$ 
即为最终答案

面对 $n\le 1e18$ 的庞大数据，问题就落到了如何快速求出 $F(n)$ 上了。根据通项公式，只需要判断 $n$ 是否有且只有一个素数非平凡因子 $p$ 即可

Pollard-Rho算法是一种用于因数分解整数的快速随机算法，它可以实现在 $n^{\frac{1}{4}}$ 的时间下随机找到一个 $n$ 的非平凡因子
其中用到了Miller-Rabin 素性测试算法，是一种测试素数的快速随机算法

Pollard-Rho算法的具体实现可以参考这篇博文：[大数因数分解Pollard_rho 算法详解](https://blog.csdn.net/maxichu/article/details/45459533)

最终根据查找结果，结合通项公式判断输出即可

## 参考代码
（Pollard_Rho算法及Miller_Rabin算法部分略）
```cpp
ll fac=0;
bool prop(ll n){//判断n==p^i
    if(n==1) return true;
    if(Miller_Rabin(n)){
        if(fac==0) fac=n;
        else if(fac!=n) return false;
        return true;
    }
    ll p=n;
    while(p==n) p=Pollard_Rho(n);
    if(prop\(p\)&&prop(n/p)) return true;
    return false;
}
void solve()
{
    srand(time(0));
    fac=0;
    ll n;cin >> n;
    if(n==1) {cout << 1 << ' ';return;}
    if(Miller_Rabin(n)) {cout << Get_Mod(n) << ' ';return;}
    if(prop(n)) cout << Get_Mod(fac) << ' ';
    else cout << 1 << ' ';
}
```
![Alt text](image-3.png)

***

# 1010.Kong Ming Qi
**~~猜结论题~~ 找规律题**
## 题目大意
如题，游戏规则和孔明棋相似
在一个 $(n+2)\times(m+2)$ 大小的棋盘的中间 $n\times m$ 的格子里放置着一些棋子
每次行动可以选择一个有四面相邻的棋子，使其跳过相邻的棋子，并使被跳过的棋子从棋盘上被移除
问对于给定的 $n,m$ ，剩余棋子个数最少是多少

## 解题思路
首先考虑只有一行/一列的情况，每次只能选择倒数第二个跳出。假设 $n=1$ ，连续模拟几次可知一定剩余 $\lceil m/2 \rceil$ 颗棋子
![Img](/images/ACM/2023Summer_HDU04_1010_1.png)

对于多行多列的情况，可以考虑以下两个基础操作：
1. 每次消去 $1\times 3$ 区域的棋子(需要左下方有棋子，中间操作)
![Img](/images/ACM/2023Summer_HDU04_1010_2.png)
2. 每次消去 $2\times 3$ 区域的棋子(最终操作)
![Img](/images/ACM/2023Summer_HDU04_1010_3.png)

连续操作 $1$ 后操作 $2$ ，即可在行数/列数大于 $3$ 的情况下任意消除 $3$ 行/列

考虑小情况 $2\times 2,2\times 3,3\times 3,2\times 4,3\times 4$ 等，借助上述基础操作以及模拟可得，当 $m,n$ 中有一个为3时，最少剩余 $2$ 颗棋子，其他情况最少剩余 $1$ 颗棋子，依此打表

## 参考程序
```cpp
void solve()
{
    ll m,n;
    cin >> m >> n;
    if(n>m) swap(m,n);
    if(n==1) cout << (m+1)/2 << endl;
    else if(!(n%3&&m%3)) cout << 2 << endl;
    else cout << 1 << endl;
}
```

***

# 1011.Circuit
**图论-最短路-Floyd**
## 题目大意
给定一个无重边、无自循环的带权有向图 $G=(V,E),|V|=n,|E|=m$ 
求 $G$ 的所有回路中最短回路的长度以及个数，个数取模

## 解题思路
全图范围的最短路计数，考虑使用弗洛伊德算法//

为了不重不漏的计数，考虑每个最短回路仅在其最大标号处计数
对于一条有向回路，可以将其最大编号到下一点的有向边单独分割出来，剩余部分即为从起点到回路上最大编号点的最短路//

借助Floyd算法，在 $O(n^3)$ 的时间内，计算最短路的同时，可以进行最短路方案数的计数，这里详细阐述一下计数过程

原图 $G$ 存在有向边 $u\rightarrow v$ ，则 $u\rightarrow v$ 的初始方案数置为 $1$ ，没有边则置为 $0$

加入点 $k$ 时，过点 $k$ 的 $i\rightarrow j$ 的最短路的方案数为： $i\rightarrow k$ 的最短方案数 $\times$ $k\rightarrow j$ 的最短路方案数
如果发生最短路更新，则方案数一并更新//另判断如果长度一致，则方案数需相加

每次完成更新后，假设 $k$ 已经是以 $i$ 为起点的最短回路上的最大编号点了，此时原图 $G$ 中如果有 $k\rightarrow i$ 的有向边，则可以构成回路，比较计数即可//

更加具体的内容可以参考代码注释

温馨提示： $INF$ 的值需要设置到 $1e18$ ，因为两段方案数乘积最大可以为 $MOD^2 \approx 1e18$ ，补题时因为 $INF$ 的值不够大卡了两个小时左右

### 时间复杂度
Floyd算法：$O(n^3)$

## 参考代码
```cpp
const long long INF=1e18;
#define N 505
ll dist[N][N]={0},cnt[N][N]={0},ori[N][N]={0};
ll mindist=INF,cntmin=0;
void RESET_G(ll n){
    FORLL(i,1,n)
        FORLL(j,1,n){
        ori[i][j]=0;
        dist[i][j]=(i==j?0:INF);
        cnt[i][j]=0;
        }
    mindist=INF,cntmin=0;
}//重置
void Floyd(ll n){
    FORLL(k,1,n)
        FORLL(i,1,n){
            FORLL(j,1,n){//Floyd
                if(dist[i][j]>dist[i][k]+dist[k][j]){
                    dist[i][j]=dist[i][k]+dist[k][j];
                    cnt[i][j]=mul(cnt[i][k],cnt[k][j]);//cnt[i][j]=cnt[i][k]*cnt[k][j]
                    //方案数为两段方案数的乘积，更新同步
                }else if(dist[i][j]==dist[i][k]+dist[k][j]){
                    addto(cnt[i][j],mul(cnt[i][k],cnt[k][j]));//cnt[i][j]+=cnt[i][k]*cnt[k][j]
                    //相等则方案数相加
                }
            }
            if(i<k&&ori[k][i]){//假设到k为以i为起点的单向环上最大点
                if(ori[k][i]+dist[i][k]<mindist) {mindist=ori[k][i]+dist[i][k];cntmin=cnt[i][k];}
                else if(ori[k][i]+dist[i][k]==mindist) addto(cntmin,cnt[i][k]);//cntmin+=cnt[i][k]
        }
    }
}
void solve()
{
    ll n,m;
    cin >> n >> m;
    RESET_G(n);
    ll u,v,w;
    FORLL(i,1,m){
        cin >> u >> v >> w;
        ori[u][v]=w;
        dist[u][v]=w;
        cnt[u][v]=1;
    }
    Floyd(n);
    if(cntmin) cout << mindist << ' ' << cntmin << endl;
    else cout << "-1 -1" << endl;
}
```

***

# 1012.a-b Problem
**贪心**
## 题目大意
给定一堆 $n$ 个石子，每个石子有非负值 $a_i,b_i$ ，分别代表Alice和Bob取该石子可以得到的分数
两人都希望自己的得分最大化，Alice执先，求最终Alice和Bob的分数差
## 解题思路
取某石子的价值不仅在于自身得分，也在于让对手失去他那部分的分值。因此根据每颗石子的分数之和排序，从大到小依次取即可
## 参考代码
```cpp
void solve()
{
    ll n;
    cin >> n;
    vector<pair<ll,pll>> v;
    pair<ll,pll> t;
    FORLL(i,1,n){
        cin >> t.second.first >> t.second.second;
        t.first=t.second.first+t.second.second;
        v.emplace_back(t);
    }
    SORT(v);
    ll A,B;A=B=0;
    for(ll i=n-1;i>=0;i--){
        if((n-1-i)&1) B+=v[i].second.second;
        else A+=v[i].second.first;
    }
    ll re=A-B;
    cout << re << endl;
}
```