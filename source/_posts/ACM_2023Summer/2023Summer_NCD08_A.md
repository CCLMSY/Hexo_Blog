---
title: 题解|A.Alive Fossils-2023暑期牛客多校08
date: 2023/8/11
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: b57662bf
description: 题解|A.Alive Fossils-2023暑期牛客多校08 Idx:1 签到
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
## 题目大意
给定 $n$ 个字符串集，求它们的交集，按字典序输出

## 解题思路
逐一处理字符串集，开个 map 记录此前的交集，从当前集合中选走元素即可

### 时间复杂度
$O(n)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

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