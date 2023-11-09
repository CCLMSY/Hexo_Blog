---
title: 题解|2023暑期牛客多校06
date: 2023/8/4
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: d9c7e69f
description: 题解|2023暑期牛客多校06
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

# A.Tree
**图论-Kruskal、动态规划**
## 题目大意
给定一棵 $n$ 个节点带点权和边权的无根树

节点具有颜色白（$0$）和黑（$1$），颜色可反转，所需代价 $cost_i$ 为该点点权

整棵树的 $earning$ 为 $\sum\limits_{u\in V_0}\sum\limits_{v\in V_1} val(u,v)$ 。其中， $val(u,v)$ 为节点 $u\rightarrow v$的最短路径上的最大边权， $V_0$ 为白色点集， $V_2$ 为黑色点集

可以操作反转节点颜色任意次，求最大 $earning-\sum cost$ （以下简称 $score$ ）

## 解题思路
注意到对于每对黑白点对，其贡献为最短路径上的最大边权。考虑利用Kruskal算法对树进行重构，即按边权从小到大的顺序进行加边

可以发现，在加入某条边 $e_i$ 时，原本在同一连通分量中的黑白点对的 $score$ 不受影响。由于加边顺序，新加的边一定具有目前最大的边权，因此只有经过新加的这条边的点对才对 $score$ 具有贡献，每个点对的贡献值为 $w_i$ ，点对数量为：$左白\times右黑+左黑\times右白$

构造dp数组：定义 $dp_{i,j}$ 为连通分量 $i$ （以并查集中连通分量的根节点标识）中具有 $j$ 个白色节点时的最大 $score$ 。初始对于点 $i$ ，$dp_{i,color_i}=0$ （不变）；$dp_{i,color_i\oplus 1}=-cost_i$ （反转）

在合并 $A、B$ 两个连通分量到 $C$ 时具有以下转移方程：
$$
dp_{C,i}=\max\limits_{0\le k \le |A| \And 0\le i-k\le |B|}\{dp_{A,k}+dp_{B,i-k}+w(k(|B|-(i-k))+(i-k)(|A|-k))\}
$$
（两边原有的 $score$ 加上过新加边的 $score$ ）

可以结合代码注释进行理解


### 时间复杂度
$O(n^2\log n)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
struct edge{
    int u,v;ll w;

    edge():u(0),v(0),w(0){}
    edge(int _u,int _v,ll _w): u(_u),v(_v),w(_w) {}

    bool operator<(const edge &b)const{
        return w<b.w;
    }
};//手写edge类，自定义比较方式
//下面的注释中也添加了不用手写的代码，优点是少写一段代码，缺点是表述较为冗杂不直观
#define N 3005
int dsu[N]={0},sz[N]={0},color[N]={0};
vector<ll> dp[N];//dp_i,j:集合i有j个白色点时的score
vector<edge> Edge;
//vector<pair<ll,pll>> Edge;//<w,<u,v>>;
vector<ll> cost(N);
ll find_dsu(ll x){
    return dsu[x]==x?x:dsu[x]=find_dsu(dsu[x]);
}//并查集
void merge_dsu(ll a,ll b,ll w){
    a=find_dsu(a);b=find_dsu(b);
    if(sz[a]>sz[b]) swap(a,b);
    if(a==b) return ;

    ll n=sz[a]+sz[b],rt;
    //n:a,b点集体积之和，即白点数量上限
    vector<ll> tmp(n+1);
    FORLL(i,0,n){
        rt=-INF;
        for(ll k=0;k<=sz[a];k++) if(k<=i&&i-k<=sz[b]){//k:a中白点数量
            ll cnt0=i-k,cnt1=sz[b]-(i-k);//cnt0:b中白点数量 cnt1:b中黑点数量
            rt=max(rt,dp[a][k]+dp[b][i-k]+w*(k*cnt1+cnt0*(sz[a]-k)));
            //dp=max(两点集已经具有的最大earning-cost+经过这条边的点对数*边权)
        }
        tmp[i]=rt;
    }
    
    dp[b]=tmp;

    sz[b]+=sz[a];
    dsu[a]=b;

}
void solve()
{
    ll n,t;cin >> n;
    Edge.resize(n-1);
    ll u,v,w;
    FORLL(i,1,n){
        dp[i].resize(2);//接下来有需求会扩容
        dsu[i]=i;sz[i]=1;
    }
    FORLL(i,1,n) cin >> color[i];
    FORLL(i,1,n){
        cin >> cost[i];
        dp[i][color[i]]=(ll)0;
        dp[i][color[i]^1]=(ll)-cost[i];
    }
    for(auto &e:Edge){
        cin >> e.u >> e.v >> e.w;
        //cin >> u >> v >> w;
        //e.second.first=u;e.second.second=v;e.first=w;
    }//存边
    SORT(Edge);//按边权从小到大重构树
    edge e;
    //pair<ll,pll> e;
    FORLL(i,0,n-2){
        e=Edge[i];
        u=e.u;v=e.v;w=e.w;
        //u=e.second.first;v=e.second.second;w=e.first;
        merge_dsu(u,v,w);
    }auto tmp=dp[find_dsu(1)];
    cout << *max_element(ALL(tmp)) << endl;
}
```

***


# B.Distance
**数学、贪心(?)**
## 题目大意
对于两个大小相同的多重集 $\mathbb{A},\mathbb{B}$ ，可以选择其中任一元素 $x$ 执行操作 $x=x+1$  任意次数，最少的使得 $\mathbb{A},\mathbb{B}$ 相同的操作次数记为 $C(\mathbb{A},\mathbb{B})$ 
不同大小的 $\mathbb{A},\mathbb{B}$ 视为 $C(\mathbb{A},\mathbb{B})=0$

现在，给定两个大小为 $n$ 的多重集 $\mathbb{S},\mathbb{T}$ ，求对于 $\mathbb{S},\mathbb{T}$ 的所有子集 $\mathbb{A},\mathbb{B}$ ，最少操作次数之和 $\sum\limits_{\mathbb{A} \subseteq \mathbb{S}}\sum\limits_{\mathbb{B} \subseteq \mathbb{T}} C(\mathbb{A},\mathbb{B})$ 的值
具有相同值的两个元素视为不同元素，答案取模

## 解题思路
对于某对子集 $\mathbb{A},\mathbb{B}$ ，为了使他们相同的操作次数最少，我们会将他们排序的元素后一一对应，使每一对中较小的数变成较大的数//假设 $a_i$ 与 $b_i$ 对应，他们在这次变化中贡献的操作次数显然是 $|a_i-b_i|$ 

那么换一种角度考虑，对于原多重集 $\mathbb{S},\mathbb{T}$ ，任取一对数 $a_i,b_j$ ，考虑它们俩对应的方案数 $cnt_{i,j}$ ，那么它们在全部方案中贡献的总操作次数即为 $|a_i-b_i|\times cnt_{i,j}$

由于我们的操作策略是排序后对应，因此先对 $\mathbb{S},\mathbb{T}$ 进行排序//
选定两个数 $a_i,b_j$ 后，它们在 $\mathbb{S},\mathbb{T}$ 中的位置前面选 $k$ 对数的方案数为 $\sum\limits_{k=0}^{min(i-1,j-1)}C_{i-1}^kC_{j-1}^k=C_{i+j-2}^k$ （[范德蒙德卷积](https://oi-wiki.org/math/combinatorics/vandermonde-convolution/)）

同理，它们在 $\mathbb{S},\mathbb{T}$ 中的位置后面选 $k$ 对数的方案数为 $C_{2*n-i-j}^k$
总方案数为 $cnt_{i,j}=C_{i+j-2}^kC_{2*n-i-j}^k$ ，乘以两数之差的绝对值即为它们对答案的总贡献//

预处理组合数，枚举 $i,j$ 求和即可

### 时间复杂度
$O(n^2)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
#define N 2005
void solve()
{
    ll n,t;cin >> n;
    vector<ll> a(n),b(n);
    for(auto &x:a) cin >> x;
    for(auto &x:b) cin >> x;
    ll re=0;SORT(a);SORT(b);
    FORLL(i,0,n-1) FORLL(j,0,n-1)
        addto(re,mul(abs(a[i]-b[j]),mul(Get_Combination(i+j,i),Get_Combination((n-i-1)+(n-j-1),(n-i-1)))));
    cout << re << endl;
}
```

***

# C.idol!!
**数学**
## 题目大意
正整数 $n$ 的双阶乘 $n!!$ 表示不超过 $n$ 且与 $n$ 有相同奇偶性的所有正整数乘积
求对于给定 $n$ ，$\prod\limits_{i=1}^n i!!$ 的后缀 $0$ 个数

## 解题思路
根据双阶乘的性质，可以得到： $(n-1)!!\times n!!=n!$
因此对于给定的 $n$ ，原式可化为：
$$\prod\limits_{i=1}^n i!!=\begin{cases}
    \prod\limits_{i=1}^\frac{n}{2} (2i)! &,n为偶数 \newline
    \prod\limits_{i=1}^\frac{n+1}{2} (2i-1)! &,n为奇数 
\end{cases}$$
显而易见的，阶乘中因子 $2$ 的个数一定多于因子 $5$ 的个数，因此题目等价于求上式中因子 $5$ 的个数//

考虑某单一阶乘 $n!$ 中所含因子 $5$ 的个数。
可以发现，每个 $5$ 的倍数项会提供 $1$ 个因子 $5$ ，共有 $\lfloor \dfrac{n}{5} \rfloor$ 项
除此之外每个 $25=5^2$ 的倍数项会额外提供一个因子 $5$ ，共有 $\lfloor \dfrac{n}{5^2} \rfloor$ 项
再除此之外每个 $125=5^3$ 的倍数项会额外提供一个因子 $5$ ，共有 $\lfloor \dfrac{n}{5^3} \rfloor$ 项……
因此对于单一阶乘 $n!$ ，其提供因子 $5$ 的数量 $cnt_5=\sum\limits_{i=1}^N \lfloor \dfrac{n}{5^i} \rfloor (5^N>n)$ 

接着考虑连乘积中因子 $5$ 个数的总和。
$$
ans=\begin{cases}
    \sum\limits_{i=1}^\frac{n}{2} \sum\limits_{j=1}^N \lfloor \dfrac{2i}{5^j} \rfloor=\sum\limits_{i=1}^N \sum\limits_{j=1}^\frac{n}{2} \lfloor \dfrac{2j}{5^i} \rfloor &,n为偶数 \newline
    \sum\limits_{i=1}^\frac{n+1}{2} \sum\limits_{j=1}^N \lfloor \dfrac{2i-1}{5^j} \rfloor=\sum\limits_{i=1}^N \sum\limits_{j=1}^\frac{n+1}{2} \lfloor \dfrac{2j-1}{5^i} \rfloor &,n为奇数 
\end{cases} \newline
$$

对于某一 $i$ ，发现不论 $n$ 的奇偶， $j=1$ 开始的每 $5^i$ 项之和构成公差为 $2\times5^i$ 的等差数列//
例：$i=1$ ，$n$ 为偶数且足够大时，$\lfloor \dfrac{2j}{5^i} \rfloor$ 的前 $15$ 项如下，其中每 $5$ 项之和构成公差为 $5\times 2$ 的等差数列： $0,0,1,1,2||2,2,3,3,4||4,4,5,5,6……$

经计算，对于某一 $i$ ，等差数列的首项为
$$
a_1=\begin{cases}
    \lfloor \dfrac{5^i}{2} \rfloor+2 &,n为偶数 \newline
    \lfloor \dfrac{5^i}{2} \rfloor+1 &,n为奇数
\end{cases}
$$

完整的段用等差数列求和，非完整的段手算一下//
​
若此前完整段的数量记为 $m$ ，则非完整段：
前 $\lfloor \dfrac{5^i}{2} \rfloor$ 项的值为 $2m$ ，
第 $\lfloor \dfrac{5^i}{2} \rfloor+1$ 至 $2\times\lfloor \dfrac{5^i}{2} \rfloor $ 项的值为 $2m+1$（手搓一下就知道了）

求和即可

令 $N=\lfloor \log_5n \rfloor+1$ ，对 $i\in[1,N]$ 遍历求和得到答案

由于答案数据极其庞大，超出了C++ %lld(64bits)的范围，因此需要使用更高位数的整数类型（如int128）//或者直接转战Python

### 时间复杂度
$O(\log n)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```python
import math
# while 1:
n=int(input())
N=int(math.log(n,5)+1)
re=0
if n%2==0 :
    for i in range(1,N+1) :
        #print("i="+str(i))
        a1=(5**i)//2+2 #首项
        #print("a1="+str(a1))
        d=(5**i)*2 #公差
        #print("d="+str(d))
        m=(n//2)//(5**i) #完整段数
        #print("m="+str(m))
        re+=(2*a1+(m-1)*d)*m//2 #完整段等差数列求和
        #print("re1:" + str(re))
        re+=(n//2-m*(5**i))*2*m #最后一段余项求和
        #print("re2:" + str(re))
        #print("pl1=" + str((n//2-m*(5**i))*2*m))
        if n//2-m*(5**i)>(5**i)//2 :
            re+=n//2-m*(5**i)-(5**i)//2
            #print("pl2=" + str(n//2-m*(5**i)-(5**i)//2))
                

if n%2 :
    for i in range(1,N+1) :
        #print("i="+str(i))
        a1=(5**i)//2+1 #首项
        #print("a1="+str(a1))
        d=(5**i)*2 #公差
        #print("d="+str(d))
        m=((n+1)//2)//(5**i) #完整段数
        #print("m="+str(m))
        re+=(2*a1+(m-1)*d)*m//2 #完整段等差数列求和
        #print("re1:" + str(re))
        re+=((n+1)//2-m*(5**i))*2*m #最后一段余项求和
        #print("re2:" + str(re)) 
        #print("pl1=" + str(((n+1)//2-m*(5**i))*2*m))
        if (n+1)//2-m*(5**i)>(5**i)//2 :
            re+=(n+1)//2-m*(5**i)-(5**i)//2
            #print("pl2=" + str((n+1)//2-m*(5**i)-(5**i)//2))
    
print(re)
```

***

# E.Sequence
**思维题**
## 题目大意
给定一个长度为 $n$ 的正整数序列，并进行 $q$ 次询问
每次询问给定一个范围 $[l,r]$ 和一个正整数 $k$ 
问能否将序列中给定范围内的子序列划分为 $k$ 段非空区间，且每段区间之和为偶数

## 解题思路
首先对于给定区间：
1. 给定区间内总元素个数不足 $k$ ，则无法划分
2. 给定区间内奇数元素个数为奇数，则给定区间的和为奇数，无法划分为 $k$ 个和为偶数的区间
3. 给定区间内奇数元素个数为偶数，则最优划分为：从前往后奇数两两匹配形成区间，余下的偶数自成一个区间

因此本题的关键就在于区间内奇数的处理

输入的记录奇数所在的位置，每次询问对于给定的区间，二分查找第一次出现奇数的位置和最后一次出现的位置，判断奇数个数
符合要求再进行区间计数判断，具体实现和解释可以参考代码注释

### 时间复杂度
2023/8/5：纠正：最坏时间复杂度为 $O(qn)$ ，卡一下平均能过qwq

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
#define N 100005
void solve()
{
    ll n,q,t;cin >> n >> q;
    vector<ll> v,odd;
    FORLL(i,1,n){
        cin >> t;v.emplace_back(t);
        if(t&1) odd.emplace_back(i);
    }//奇数下标存入odd,O(n)
    ll l,r,k;
    FORLL(i,1,q){
        cin >> l >> r >> k;
        if(r-l<k-1) {cout << NO;continue;}//元素数量小于k
        if(odd.empty()) {cout << YES;continue;}//整个序列无奇数
        auto ol=lower_bound(ALL(odd),l);//找到区间左端点右边最近的奇数的位置
        if((upper_bound(ALL(odd),r)-ol)&1) {cout << NO;continue;}
        //如果区间内奇数个数为奇数个，一定会残留一个区间和为奇数的区间
        ll cnt=0;//最多区间计数
        while(ol!=odd.end()&&*ol<=r){//下一个奇数在区间内
            cnt+=*ol-l;ol++;//奇数左边的偶数一个记一段
            if(*ol<=r) cnt++;//两个奇数之间记一段
            l=*ol+1;ol++;//更新左端点
        }if(ol==odd.begin()) {cout << YES;continue;}
        ol--;cnt+=r-*ol;//加上区间右边剩余偶数
        if(cnt>=k) cout << YES;
        else cout << NO;
    }
}
```

***

# G.Gcd
**数论**
## 题目大意
给定一个包含两个非负数的初始集合 $S=\{x,y\}$ 
每次操作可以选定其中不相等的两个数 $a,b$ ，并将 $a-b$ 或 $gcd(a,b)$ 置入集合 $S$ ，其中 $gcd(0,a)=a$
可以操作任意次，问能否使得集合 $S$ 包含非负数 $z$

### 前置知识点
[裴蜀定理](https://oi-wiki.org/math/number-theory/bezouts/)
## 解题思路
根据裴蜀定理，两个正整数辗转相减只能得到他们最大公约数的倍数//
因此对于 $z$ ，判断其是否是 $g=gcd(x,y)$ 的倍数即可。
如果 $z$ 是 $g$ 的倍数，则可以通过以下操作得到 $z$ ：
1. 将 $g=gcd(x,y)$ 置入集合
2. $x$ 作为 $g$ 的倍数，其加减任意次 $g$ 便可得到任意 $g$ 的倍数。
   只能减不能加怎么办呢//先把 $x$ 减到 $-g$ 就好了

值得注意的是，本题的数据约束为**非负数**，这意味着需要对 $0$ 的情况进行特判//
1. 对于 $z=0$ ，仅当 $x,y$ 有 $0$ 时有解
2. 对于 $x=0$ 或 $y=0$ ，仅当 $z$ 为非 $0$ 项的倍数时有解（实际上这条也满足裴蜀定理，直接归入一般情况即可）

参考样例：
```
in: 1 0 1 2
out: YES
```

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll x,y,z;
    cin >> x >> y >> z;
    if(x&&y&&z==0) {cout << NO;return;}
    ll g=gcd(x,y);
    if(z%g) cout << NO;
    else cout << YES;
}
```