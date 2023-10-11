---
title: 题解|E.Sequence-2023暑期牛客多校06
date: 2023/8/4
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 思维
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: '72148038'
description: 题解|E.Sequence-2023暑期牛客多校06 Idx:2 思维 签到
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