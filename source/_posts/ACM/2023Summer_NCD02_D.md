---
title: 题解|D.The Game of Eating-2023暑期牛客多校02
date: 2023/7/21
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 贪心
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 9f517358
description: 题解|D.The Game of Eating-2023暑期牛客多校02 Idx:5 贪心
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

# D.The Game of Eating
**贪心**
## 题目大意
$n$ 个人聚餐， 在 $m$ 道菜品中选择 $k$ 道，且不能重复。从1号开始，每个人轮流选择一道菜
每个人对于每个菜品都有一个喜爱值，第 $i$ 个人对第 $j$ 个菜品的喜爱程度记为 $a_{i,j}$ 
假设每个人都想要使 自己对最终点的 $k$ 道菜的喜爱值总和 最大，求最终选定的菜单
## 解题思路
对于当前点菜的人，假设他最喜欢的菜还没有被点，但他仍然需要考虑后面是否有人选择这道菜。如果后面一定有人选择这道菜，他可以选择第二喜欢的菜品以使得自己的满意度最大化
基于这种情况分析，最后一个点菜的人无需考虑其他人的需求，只需要选择剩余菜品中最喜欢的那一个，即可使得自己的满意度最大化
假设所有菜品中他最喜欢的还没有被选取，那么就会选择这个菜品。反推到倒数第二个、第三个亦此

因此，只需要将点菜的顺序逆转，这样只要每个人选择剩余菜品中最喜欢的菜品即可，无需考虑其他人的需求。

### 时间复杂度
$k$ 次遍历菜品： $O(km)$
## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
int solve()
{
    ll m,n,k,t;
    cin >> n >> m >> k;
    vector<vector<pll>> v(n+1);
    vector<ll> re;
    vector<pll>::iterator it;
    vector<bool> chosen(m+1,false);
    FORLL(i,1,n){
        FORLL(j,1,m){
            cin >> t;
            v[i].emplace_back(t,j);
        }
        SORT(v[i]);
    }
    ll p=(k-1)%n+1;
    while(k--){
        it=v[p].end();--it;
        while(chosen[(*it).second]) it--;
        re.emplace_back((*it).second);
        chosen[(*it).second]=true;
        p--;
        if(p==0) p=n;
    }
    SORT(re);
    for(auto x:re){
        cout << x << ' ';
    }cout << endl;
    return 0;
}
```