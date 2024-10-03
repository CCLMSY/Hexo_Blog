---
title: 题解|2024暑期牛客多校09
date: 2024/8/13
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD09
description: 题解|2024暑期牛客多校09
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

比赛链接：[2024牛客暑期多校训练营9](https://ac.nowcoder.com/acm/contest/81604)

# A.Image Scaling
## 题意
给定由 `.` 和 `x` 组成的 $n\times m$ 的 $n\times m$ 矩阵，$x$ 部分是一个子矩阵。
提取并在长宽比不变的情况下，将子矩阵尽可能缩小并输出。

## 解题思路
模拟，缩小到 $1/gcd$

## 参考代码
```cpp
void solve()
{
    ll n,m;
    cin >> n >> m;
    ll fl=0,st=-1;
    ll nn=-1,mm,idx;
    string s;
    FORLL(i,0,n-1){
        cin >> s;
        if(fl) if(s[idx]!='x') {nn=i-st;break;}
        else{
            FORLL(j,0,m-1){
                if(s[j]=='x'){
                    fl=1; st=i; idx=j;
                    ll t=j;
                    while(t<m&&s[t]=='x') t++;
                    mm=t-j;
                    break;
                }
            }
        }
    }
    if(nn==-1) nn=n-st;
    ll g=__gcd(nn,mm);
    nn/=g; mm/=g;
    FORLL(i,0,nn-1){
        FORLL(j,0,mm-1){
            cout << 'x';
        }cout << endl;
    }
}
```

# K.Kill The Monsters
## 题意
$n$ 个怪兽，第 $i$ 个怪兽的体力为 $a_i$ 。

每次可以进行一种操作：
1. 所有怪兽体力 $-1$
2. 选择一个怪兽 $i$ 使得 $a_i\leftarrow \lfloor \dfrac{a_i}{k} \rfloor$

问最少多少次操作可以使所有怪兽的体力都为 $0$ 。

## 解题思路
贪心的，先连续对最大体力的怪兽进行第二种操作，再进行第一种操作。
用优先队列维护最大体力。
记当前已经操作了 $cnt$ 次，用 $a_{max}+cnt$ 维护最小操作次数。

## 参考代码
```cpp
void solve()
{
    ll n,k,t;cin >> n >> k;
    priority_queue<ll> pq;
    FORLL(i,1,n) {cin >> t;pq.push(t);}
    ll cur=0,ans=pq.top();
    if(k==1) {cout << ans << endl;return;}
    while(pq.top()>1){
        t=pq.top();pq.pop();
        t/=k; cur++;
        pq.push(t);
        chmin(ans,pq.top()+cur);
    }
    cout << ans  << endl;
}
```
