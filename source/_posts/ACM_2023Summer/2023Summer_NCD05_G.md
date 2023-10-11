---
title: 题解|G.Go to Play Maimai DX-2023暑期牛客多校05
date: 2023/7/31
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
abbrlink: f23ccccf
description: 题解|G.Go to Play Maimai DX-2023暑期牛客多校05 Idx:1 快慢指针模拟
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

# G.Go to Play Maimai DX
**签到**
## 题目大意
给定一个长度为 $n$ 仅含有 $1,2,3,4$ 四种数字的序列
求最短的包含 $1,2,3$ 和 $k$ 个 $4$ 的区间的长度
## 解题思路
用快慢指针确定区间
快指针每次记录元素，直到满足条件；
慢指针每次移除元素，直到不满足条件
即可得到每个满足条件的最小区间，比较长度即可
### 时间复杂度
$O(n)$
## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
#define N 100005
int v[N]={0};
void solve()
{
	ll n,k;cin >> n >> k;
    FORLL(i,1,n) cin >> v[i];
    ll l=0,r=0,cnt[5]={0},re=n;
    while(r<=n){
        while(!(cnt[1]&&cnt[2]&&cnt[3]&&cnt[4]>=k)){
            r++;cnt[v[r]]++;if(r>n) break;
        }
        while(cnt[1]&&cnt[2]&&cnt[3]&&cnt[4]>=k){
            l++;cnt[v[l]]--;if(l>r) break;
        }re=min(re,r-l+1);
    }cout << re << endl;
}
```