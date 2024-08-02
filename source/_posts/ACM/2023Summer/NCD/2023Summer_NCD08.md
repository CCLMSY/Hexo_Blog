---
title: 题解|2023暑期牛客多校08
date: 2023/8/11
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_NCD08
description: 题解|2023暑期牛客多校08
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

# A.Alive Fossils
**签到**
## 题意
给定 $n$ 个字符串集，求它们的交集，按字典序输出

## 解题思路
逐一处理字符串集，开个 map 记录此前的交集，从当前集合中选走元素即可

### 时间复杂度
$O(n)$

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;ll tn;cin >> tn;
    map<string,int> mp;string ts;
    vector<string> v;
    FORLL(i,1,tn){
        cin >> ts;
        mp[ts]++;
        v.emplace_back(ts);
    }
    FORLL(i,2,n){
        v.clear();
        cin >> tn;
        FORLL(j,1,tn){
            cin >> ts;
            if(mp[ts]) v.emplace_back(ts);
        }
        mp.clear();
        for(auto s:v) mp[s]++;
    }
    SORT(v);
    cout << v.size() << endl;
    for(auto s:v) cout << s << endl;
}
```

***

# J.Permutation and Primes
**构造**
## 题意
给定一个正整数 $n$ ，构造一个 $n$ 的排列，使得每对相邻元素的和或差的绝对值为一奇素数

## 解题思路
两个数的和或差是奇数，那么它们的奇偶性一定是不同的，因此所求排列中，奇数和偶数是交错分布的

对于排列的构造，首先考虑能不能通过自然排列转化而成//

赛时首先想到奇偶分开后，将全体偶数向左或向右循环移动 $2$ ~ $3$ 位，这样可以保证绝大部分位置和左右两边之差为 $3$ 或 $5$ //当时以为要切了，结果剩余的数字考虑了两年半还是没有找到符合题意而不失一般性的构造方法

然后考虑分段调整。在上面思想的基础上，发现如果元素个数为 $8$ 个，那么全体偶数左移 $2$ 位和右移 $2$ 位的结果都为： $1,6,3,8,5,2,7,4$ 。并且如果下一段长度为 $8$ 的序列也按照这种规则变换顺序，那么 $4$ 和下一段长度为 $8$ 的序列的开头 $9$ 之差为 $5$ ，符合题意，从而解决了边界问题

将 $n$ 模 $8$ 取余，记 $n=8k+r$ 。解决前 $r$ 个数的排列后，之后每 $8$ 个数按 $1,6,3,8,5,2,7,4$ 的顺序变换即可

### 时间复杂度
$O(n)$

## 参考代码
```cpp
ll rem[8][8]={{},{1},{1,2},{1,2,3},{1,4,3,2},{5,2,1,4,3},{5,2,1,4,3,6},{7,2,5,6,3,4,1}};
ll pl[9]={0,1,6,3,8,5,2,7,4};
void solve(){
    ll n;cin >> n;
    ll r=n%8,d=n/8;
    vector<ll> v;
    FORLL(i,0,r-1) v.emplace_back(rem[r][i]);
    FORLL(i,0,d-1){
        FORLL(j,1,8){
            v.emplace_back(8*i+pl[j]+r);
        }
    }
    FORLL(i,0,n-1) cout << v[i] << Presentation(i,n-1);
}
```