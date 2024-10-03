---
title: 题解|2024暑期杭电多校05
date: 2024/8/2
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU05
description: 题解|2024暑期杭电多校05
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（5）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A85%A3%A9&source=1&searchmode=source)

# (1002)HDU7482 Array-Gift
## 题意
给定一个长度为 $n$ 的正整数数组 $a$ ，提供以下2种操作：
1. 选择两个不同的下标 $i,j$ ，$a_i\leftarrow a_i\ mod\ a_j$
2. 选择一个下标 $i$ 和一个任意正整数 $x$ ，$a_i\leftarrow a_i+x$

求使得数组仅有1个非0元素的最小操作次数。

## 解题思路
add：做之前先对数组去重，因为重复的数可以在开始就通过操作1变为0。

考虑最坏情况的操作次数：
选定两个不同的下标 $i,j$ ，不妨设 $a_i<=a_j$ 。（1次）
做操作2使得 $a_i=a_j+1$ ，再做操作1使得 $a_i=1$ 。（1次）
最后对每个 $k\neq i$ 做操作1：$a_k\leftarrow a_k\ mod\ a_i=0$ 。（$n-1$次）
最坏情况的操作次数为 $n+1$ 。

最好的情况：存在一个下标 $i$ 使得 $a_i$ 能够整除数组中所有元素。
满足这个条件的 $a_i$ 一定是数组中的最小值。
这种情况只需对每个 $k\neq i$ 做操作1：$a_k\leftarrow a_k\ mod\ a_i=0$ ，操作次数为 $n-1$ 。

由于最坏情况难以确定，考虑操作次数为 $n$ 的情况：
1. 数组 $a$ 中的最小值经过操作2 ，$\min(a)\leftarrow\min(a)+x$ ，能够整除数组中所有元素。
2. 某个元素经过操作1，能够整除数组中所有元素。

对于第二种情况，维护gcd前缀/后缀（似乎不维护也能过？）。
枚举被修改的数 $a_i$ 和模数 $a_j$ ，若 $a_i\ mod\ a_j=\gcd\limits_k^{k\neq i}a_k$ ，则最少操作次数为 $n$ 。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    create_vec(v,n);
    SORT(v);
    vector<ll> tmp;
    for(auto x:v){
        int fl=1;
        for(auto m:tmp) if(x%m==0) {fl=0;break;}
        if(fl) tmp.emplace_back(x);
    }
    ll on=n; v=tmp; n=v.size(); //on是原数组大小
    v.insert(v.begin(),0); v.insert(v.end(),0); //起始下标改为1
    if(n==1) {cout << on-1 << endl;return;}
    
    vector<ll> preg(n+2,0),sufg(n+2,0); 
    FORLL(i,1,n) preg[i]=__gcd(preg[i-1],v[i]);
    FORLL_rev(i,n,1) sufg[i]=__gcd(sufg[i+1],v[i]);

    if(sufg[2]>v[1]&&sufg[2]<=v[2]) {cout << on << endl;return;} //剩下的数模（v1+x）为0
    FORLL(i,1,n){ //被修改的数
        ll g=__gcd(preg[i-1],sufg[i+1]);
        FORLL(j,1,i-1){//模数
            ll t=v[i]%v[j];
            if(t==0) continue;
            if(g%t==0) {cout << on << endl;return;}
        }
    }

    cout << on+1 << endl;
}
```

# (1006)HDU7486 猫罐头游戏
## 题意
有三堆猫罐头，每堆猫罐头的数量分别为 $a,b,c$ 。
两只猫猫进行博弈，每次可以选择一堆猫罐头全部吃掉，然后再从剩下的两堆中选择一堆猫罐头分成两份（每份至少一罐）。
不能操作的猫猫输掉游戏。

问先手是否有必胜策略。
## 解题思路
最终必败态是 $a=b=c=1$ 。

当开局有3个奇数时，不论怎么操作，结果都是2奇数1偶数；
当开局有1个或2个偶数时，可以控制结果为3个奇数。
三个奇数的尽头就是3个1必败，因此开局3奇数必败，开局1或2偶数必胜。

开局全偶数的情况，可以从lowbit的角度考虑。
当开局三个数lowbit位于同一位时，不论怎么操作都不能使lowbit相等。
当开局三个数lowbit位于不同位时，可以通过操作控制lowbit相等，整个过程lowbit减小。
lowbit相等的尽头是三个数的lowbit都等于$1$也就是三奇数的必败态。

因此，开局三个数lowbit相等必败。
观察到三个数不全是偶数的情况也可以归并到这个结论中，2行秒了。

## 参考代码
```cpp
void solve()
{
    ll a,b,c; cin >> a >> b >> c;
    if(lowbit(a)==lowbit(b)&&lowbit(b)==lowbit(c))  NO; else YES;
}
```

# (1011)HDU7491 开关灯
## 题意
有 $n$ 盏灯排成一排，编号为 $1\sim n$ ，初始状态为全灭。
每次操作可以选择一盏灯，反转这盏灯和与之相邻的灯的状态（开变关，关变开）。
求任意操作后，这排灯可以到达多少种不同的状态。

## 解题思路
观察样例发现答案满足 $2^n$ 。
手玩发现只有 $n\equiv2(mod\ 3)$ 时达不到全亮状态，是 $2^{n-1}$ 。

如果发现力不够，还可以考虑以下方法：
对位置1,2分别做一次操作，可以单独反转3；再对4,5分别做一次操作，可以单独反转6。
以此类推，正着做可以单独反转所有 $i\equiv0(mod\ 3)$，反着做可以单独反转所有 $i\equiv n-2(mod\ 3)$ 。
如果可以单独任意反转连续2个灯，则与它们相邻的灯也可以单独任意反转，以达到所有 $2^n$ 种状态。

当且仅当 $n\equiv2(mod\ 3)$ 时，正着做和反着做反转的都是同一批灯，无法通过以上操作任意反转所有的灯。
由于无法到达全亮状态，答案少一半，为 $2^{n-1}$ 。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    if(n%3==2) cout << qcpow(2,n-1) << endl;
    else cout << qcpow(2,n) << endl;
}
```

# (1013)HDU7493 飞行棋
## 题意
$0\sim n$ 号格子排成一排，棋子初始在 $0$ 号格，目标是精确到达第 $n$ 格。
每次随机从 $1\sim n$ 中均匀随机一个正整数 $x$ ，然后向前移动 $x$ 步，若移动到第 $n$ 格还有剩余步数，则会往回走。
如果随机到 $n$ 且最终没有精确到达第 $n$ 格，则可以再从 $1\sim n-1$ 中均匀随机一个正整数并行动。

求精确到达第 $n$ 格的期望步数。

## 解题思路
从 $0$ 号格子一次到达第 $n$ 格的概率是 $\dfrac{1}{n}$ 。
从 $1\sim n-1$ 号格子一次到达第 $n$ 格的概率都是 $\dfrac{1}{n}(1+\dfrac{1}{n-1})=\dfrac{1}{n-1}$ ，期望为 $n-1$
如果没有到达终点，棋子只会落在 $1\sim n-1$ 号格子上。

因此答案为 $\dfrac{1}{n}+\dfrac{n-1}{n}\times ((n-1)+1)=n+1+\dfrac{1}{n}$ 。
（式子中 $(n-1)+1$ 是加上了从0开始的第一步）

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    cout << sub(add(inv(n),n),1) << endl;
}
```