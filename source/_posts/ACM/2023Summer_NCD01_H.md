---
title: 题解|H.Matches-2023暑期牛客多校01
date: 2023/7/17
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
abbrlink: 89c8e174
description: 题解|H.Matches-2023暑期牛客多校01 Idx:4 思维
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

# H.Matches

**二分、包络**

## 题目大意
给定两个长度为 $n$ 的序列 $a,b$ ，定义他们之间的距离为对应元素差的绝对值之和，即：
$$
\sum\limits_{i=1}^n |a_i-b_i|
$$
可以对其中一个序列交换两个数，问至多操作一次后的最小距离。

## 解题思路
绝对值的本质就是数轴上区间的大小，把每一对元素看作一个区间，就能将问题转化。

对于某一个区间， $a_i,b_i$ 决定它自身的正反序

对于某两个区间：
覆盖区域有 相交、包络、不交 三种相对关系
正反序有 正序、反序 两种相对关系
组合之下便有6种相对关系

针对两个区间的相对关系的转换，展开以下分类讨论
1.正序相交 $\rightleftarrows$ 正序包络![Img](/images/ACM/2023Summer_NCD01_H_1.png)
2.正序不交 $\rightleftarrows$ 反序包络![Img](/images/ACM/2023Summer_NCD01_H_2.png)
3.反序不交 $\rightleftarrows$ 反序相交![Img](/images/ACM/2023Summer_NCD01_H_3.png)

可以看到绝对值减小的情况只有两种：
1.反序相交 $\Rightarrow$ 反序不交
2.反序包络 $\Rightarrow$ 正序不交
它们对距离的减少量都是重叠区间大小的两倍

综上所述，只需要找出反序相交和反序包络的情况中，重叠区间最大的情况，即可得到答案。

对于输入数据，可以先按照 $a_i,b_i$ 的相对大小，将正序区间和反序区间分别存入两个容器，记为 $S,T$ ，并算出初始值 $sum$ 
对 $S$ 进行预处理，按照区间起点排序，去除被正序包络的区间（不难证明，被正序包络的区间对答案的贡献一定不大于正序包络它的区间）
对于 $T$ 中的每个区间，二分查找 $S$ 中与它有交集的区间，求出重叠区间大小的最大值 $dif$ 

最终答案为 $sum-2\times dif$ 

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
int solve()
{
    ll n,sum=0,dif=0;
    cin >> n;
    vector<pll> v(n);
    FORLL(i,0,n-1) cin >> v[i].first;
    FORLL(i,0,n-1) cin >> v[i].second;
    vector<pll> S,T;
    vector<ll> Sx,Sy,len;
    for(auto [a,b]:v)
    {
        sum+=abs(a-b);

        if(a<b) S.emplace_back(a,b);
        if(a>b) T.emplace_back(b,a);
    }

    SORT(S);
    ll r=-INF;
    for(auto p:S)
    {
        if(p.second<=r) continue;
        r=p.second;
        Sx.emplace_back(p.first);
        Sy.emplace_back(p.second);
        len.emplace_back(p.second-p.first);
    }//消除S中被正向包络的区间。算法决定Sy递增。

    auto finddif=[&](ll x,ll y){//对于给定的区间
        ll m=Sx.size(),re=0;
        ll l=upper_bound(ALL(Sx),x)-Sx.begin();
        ll r=lower_bound(ALL(Sy),y)-Sy.begin();
        //查找该区间在S中的位置
        if(l>0) re=max(re,min(Sy[l-1],y)-x);//反序相交1
        if(r<m) re=max(re,y-max(Sx[r],x));//反序相交2
        if(r>l) re=max(re,*max_element(len.begin()+l,len.begin()+r));//反序包络
        return re;
    };

    for(auto [x,y]:T) dif=max(dif,finddif(x,y));
    cout << sum-dif*2 << endl;
    return 0;
}

```
