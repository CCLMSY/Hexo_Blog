---
title: 题解|F.Election of the King-2023暑期牛客多校04
date: 2023/7/28
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 模拟
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: b922925e
description: 题解|F.Election of the King-2023暑期牛客多校04 Idx:1 模拟
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

# F.Election of the King
**签到、模拟**
## 题目大意
$n$ 个人竞选国王，每个人的从政理念用 $a_i$ 量化表示
每人每轮可以进行投票，对于个人而言，他会将票投给和他自己的政治理念相差最大的人
票数最多的人将被淘汰。如果票数相同，则淘汰值最大的；如果最大值有多个，则淘汰序号最大的人。
重复直到剩余一人成为国王，问最后谁会竞选上国王
## 解题思路
对于其中某一个人，和他相差最大的，一定是最大值与最小值之一
那么每次一定淘汰最大值或最小值
因此对所有人进行排序，每轮投票处于中间位置的人代表着多数人的意志
因此每轮看中间的人投最大值还是最小值，踢到只剩一人即可

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll n,t;
    cin >> n;
    deque<pll> v;
    FORLL(i,1,n){
        cin >> t;
        v.emplace_back(t,i);
    }SORT(v);
    float mid;
    while(n>1){
        mid=1.*(v.front().first+v.back().first)/2;
        if(v[(n+1)/2-1].first<=mid) v.pop_back();
        else v.pop_front();
        n--;
    }cout << v.front().second << endl;
}
```