---
title: 题解|L.We are the Lights-2023暑期牛客多校04
date: 2023/7/28
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 思维
  - 模拟
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 6afb2bc8
description: 题解|L.We are the Lights-2023暑期牛客多校04 Idx:2 思维·模拟
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

# L.We are the Lights
**思维题**
## 题目大意
有 $n\times m$ 的电灯矩阵，初始全为关闭状态

每次操作会打开或关闭某一行/列的所有灯

问在给定的 $q$ 次操作后，共有多少盏灯亮着

## 解题思路
这道题和某刷墙小游戏类似（）玩过的话应该不难想到做法
![Img](/images/ACM/2023Summer_NCD04_L.jpg)
后面的行动会覆盖前面的行动，用数组记录当前行/列是否已被覆盖

从后往前遍历操作即可

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
bool rc[N]={0},op[N]={0},rowvd[N]={0},colvd[N]={0};
ll x[N]={0},cnt=0,cntr=0,cntc=0;
void solve()
{
    ll n,m,q;
    cin >> n >> m >> q;
    string trc,top;ll t;
    FORLL(i,1,q){
        cin >> trc >> t >> top;
        if(trc=="row") rc[i]=1;else rc[i]=0;
        if(top=="on") op[i]=1;else op[i]=0;
        x[i]=t;
    }
    for(ll i=q;i>0;i--){
        if(rc[i]){
            if(rowvd[x[i]]==0){
                rowvd[x[i]]=1;cntr++;
                if(op[i]==1) cnt+=m-cntc;
            }
        }else{
            if(colvd[x[i]]==0){
                colvd[x[i]]=1;cntc++;
                if(op[i]==1) cnt+=n-cntr;
            }
        }
    }cout << cnt << endl;
}
```