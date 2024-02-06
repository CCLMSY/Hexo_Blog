---
title: 题解|2023暑期牛客多校03
date: 2023/7/24
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 60ea0959
description: 题解|2023暑期牛客多校03
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

# A.World Fragments I
**字符串、签到**
## 题目大意
给定两个二进制数 $x,y$ 
每次可以选择 $x$ 中的一个数位 $d$ ,并可执行 $x=x+d$ 或 $x=x-d$
问能否通过任意次以上操作，使得 $x=y$ ，并给出最少次数

## 解题思路
这两个二进制数可以视为01串
对于 $x=0$ ，每一位都是0，除非 $y=0$ ，否则无法达成条件
对于 $x\ne 0$ ，必定有一位上是 $1$ ，通过题设操作可以转化为任何数，最小次数为这两个数的差的绝对值 $|x-y|$

bitset是一种非常便于处理01串的STL数据结构，合理利用可以省去很多麻烦

## 参考程序
```cpp
void solve()
{
    string x,y;
    cin >> x >> y;
    bitset<1000> a(x),b(y);
    ll m,n;
    if(a.none()&&b.any()) {cout << "-1" << endl;return;}
    m=a.to_ullong();
    n=b.to_ullong();
    cout << abs(m-n) << endl;
}
```

***

# D.Ama no Jaku
**思维**
## 题目大意
给定一个01矩阵，每次操作可以选定任意一行或一列并将其反转
问在操作任意次后，能否使得该矩阵 行代表的二进制数$r_i$ 与列代表的二进制数 $c_i$ ，满足 $min(r_i)\ge max(c_i)$
若能，输出最少操作次数，否则输出“-1”

## 解题思路
首先考虑满足条件的矩阵的状态
对于 $\text O$ 矩阵，$\forall i ,r_i=c_i=0$ ，一定满足条件
要使 $min(r_i)>0$ ，每一行都要有 $1$ 。如果在每一行都加上一个 $1$ ，会发现无论如何排列顺序，都会使得 $min(r_i)\lt max(c_i)$
最终当所有元素都转化为 $1$ 时，满足条件
综上，只有当元素全为 $0$ 或 $1$ 时满足条件

显然如果一个矩阵可以转化为全 $0$ 矩阵，那么它一定可以转化为全 $1$ 矩阵，因此考虑原矩阵是否可以转化为全 $0$ 矩阵

先将第一列全部转化为 $0$ ，后面每一列的值都必须全为 $0$ 或 $1$ 才可以满足条件
分别记录行和列的首位转化为0的次数，分类比较即可

## 参考代码
```cpp
void solve()
{
    ll n,cntr=0,cntc=0,re1,re2;//转置为0的次数
    cin >> n;
    bitset<2005> mat[2005];
    FORLL(i,0,n-1) cin >> mat[i];
    if(n==1) {cout << 0 << endl;return;}
    FORLL(i,0,n-1) if(mat[i][0]) {mat[i].flip();cntr++;}
    //FORLL(i,0,n-1) {FORLL(j,0,n-1) cout << mat[i][j]; cout << endl;}
    FORLL(j,1,n-1){
        int b=mat[0][j];
        if(b) cntc++;
        FORLL(i,1,n-1) if(mat[i][j]!=b) {cout << "-1" << endl;return;}
    }
    re1=cntc+(cntr>=(n+1)/2?cntr:n-cntr);
    re2=cntr+(cntc>=(n+1)/2?cntc:n-cntc);
    cout << (re1<re2?re1:re2) << endl;
}
```

***

# H.Until the Blue Moon Rises
**数学题**
## 题目大意
对于给定的 $n$ 个数，每次操作可以使一个数 $-1$ ，另一个数 $+1$ 
可以操作任意次，问是否可以使这些数全部成为质数
### 前置知识点
[哥德巴赫猜想](https://baike.baidu.com/item/%E5%93%A5%E5%BE%B7%E5%B7%B4%E8%B5%AB%E7%8C%9C%E6%83%B3/72364)：任一大于5的整数都可写成三个质数之和，任一大于2的偶数都可写成两个素数之和
## 解题思路
题设操作任意次的效果即：把所有数的和 $sum$ 分成 $n$ 个数
问是否有一种分法使得这 $n$ 个数都是质数

1. $n$ 个数都是质数时，最小情况是所有的数全为 $2$ 
   因此，当 $sum<2\times n$ 时一定不可以构成
2. $n=1$ 
   只有一个数，直接判断它是否为质数即可
3. $n=2$ 
   如果它们的和是偶数，根据哥德巴赫猜想一定可以构成；
   如果它们的和是奇数，分成两个数一定是一奇一偶，众嗦粥汁，偶数中只有2是质数，因此判断 $n-2$ 是否为质数即可
4. $n=3$ 
   由于 $sum<2\times n=6$ 的情况已经在 $1$ 中筛去，故根据哥德巴赫猜想一定可以构成
5. $n>3$
   每次分出 $2$ 直到剩余 $3$ 个数，利用上一条可知一定可以构成

## 参考代码
```cpp
bool checkprime(ll n){
    if(n<2) return false;
    if(n==2||n==3) return true;
    ll t=sqrt(n)+1;
    FORLL(i,2,t) if((n/i)*i==n){
        return false;
    }
    return true;
}
#define N 100005
void solve()
{
    ll t,sum=0;
    ll n;
    cin >> n;
    FORLL(i,1,n){
        cin >> t;
        sum+=t;
    }
    if(sum<2*n) {cout << NO ;return;}
    if(n==1){
        if(checkprime(sum))
        {
            cout << YES;
            return;
        }
        cout << NO ;
        return;
    }
    if(n==2){
        if(sum&1){
            if(checkprime(sum-2)) {cout << YES ;return;}
            cout << NO;
            return;
        }
        cout << YES;
        return;
    }
    cout << YES;
}
```

***

# J.Fine Logic
**拓扑排序**
## 题目大意
对于 $n$ 个人（记为 $1-n$），给定 $m$ 对偏序关系，表示胜利关系
对于一组排名，位于前面的人视为赢过后面的人
问如何构造最少组数的排名，使得满足给定的 $m$ 对胜利关系

## 解题思路
对于 $n$ 个人之间的所有胜利关系，构造两组排名：
$$
1,2,3,\cdots,n \newline
n,n-1,n-2,\cdots,1
$$
一定能满足所有胜利关系

如果 $m$ 个胜利关系构成DAG，则将它的拓扑排序列作为排名输出一组即可
否则输出上面两组排名

## 参考代码
```cpp
void solve()
{
    ll n,m;
    cin >> n >> m;
    vector<vector<ll>> G(n+1),pre(n+1);
    vector<pll> E;
    vector<ll>::iterator it;
    ll u,v;
    FORLL(i,1,m){
        cin >> u >> v;
        E.emplace_back(u,v);
    }
    SORT(E);
    unique(ALL(E));
    for(auto e:E){
        G[e.first].emplace_back(e.second);
        pre[e.second].emplace_back(e.first);
    }
    FORLL(i,1,n){
        SORT(G[i]);
        SORT(pre[i]);
    }
    ll cnt=0;
    int flag=1;
    vector<ll> vec;
    bitset<1000005> f;
    f.reset();
    while(flag){
        flag=0;
        FORLL(i,1,n) if(f[i]==0){
            while(pre[i].size()&&f[pre[i].back()]) pre[i].pop_back();
            if(f[i]==0&&pre[i].empty()){
                vec.emplace_back(i);
                f[i]=1;flag=1;cnt++;
            }
        }
    }
    if(cnt==n){
        cout << "1" << endl;
        for(auto i:vec) cout << i << ' ';
        cout << endl;
        return;
    }else{
        cout << "2" << endl;
        FORLL(i,1,n) cout << i << ' ';
        cout << endl;
        for(ll i=n;i>0;i--) cout << i << ' ';
        cout << endl;
    }
    
}
```