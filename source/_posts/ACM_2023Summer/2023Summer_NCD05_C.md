---
title: 题解|C.Cheeeeen the Cute Cat-2023暑期牛客多校05
date: 2023/7/31
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 图论
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 185a5ce3
description: 题解|C.Cheeeeen the Cute Cat-2023暑期牛客多校05 Idx:3 二部图最大匹配
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

# C.Cheeeeen the Cute Cat
**二部图最大匹配**
## 题目大意
给定一个具有 $2n$ 个节点的二部图，前 $n$ 个节点和后 $n$ 个节点各成一部
对于每对 $(i,j),i\ne j$ ，保证在 $i,j+n$ 和 $j,i+n$ 之间有且仅有一条边，整幅图的边数共 $C_n^2$ 条
求这个二部图的最大匹配

### 前置知识点
[二分图](https://oi-wiki.org/graph/bi-graph/)
[二分图最大匹配](https://oi-wiki.org/graph/graph-matching/bigraph-match/)

## 解题思路
题解和讨论区谈及竞赛图以及相关特点，稍微了解了一下但是赛事确实没发现可以把这个图转化成一个竞赛图//
当时一眼看去就像一道朴实无华的板子题，队友直接交了一发板子就过了//
跑一遍增广路求出最大匹配即可，具体的实现原理在OI-Wiki上有非常详细的推导与说明//
本蒟蒻就是看它学的qwq这里就不过多赘述了

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll n,t;
    cin >> n;
    augment_path G(n,n*2);
    FORLL(i,0,n-1)
        FORLL(j,0,n-1){
            cin >> t;
            if(t) G.add(i,j+n);
        }cout << G.solve() << endl;
}
```