---
title: 题解|A.Bobo String Construction-2023暑期牛客多校04
date: 2023/7/28
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 构造
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 6e0e8bd8
description: 题解|A.Bobo String Construction-2023暑期牛客多校04 Idx:3 构造
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

# A.Bobo String Construction
**构造**
## 题目大意
给定一个标识01串 $t$ ，构造一个长度为 $n$ 的信息01串 $s$ ，使得串 $m=t+s+t$ 中，串$t$ 仅在开头和结尾各出现过一次。（其中 $+$ 表示字符串连接运算）
## 解题思路
考虑构造 $s$ 全为 $0$ 或 $1$ 

对于特殊情况， $t$ 全为 $0$ 或 $1$ ，则使 $s$ 全为 $1$ 或 $0$ 即可

如果 $t$ 中兼有 $0$ 和 $1$ ，则 显然 $s$ 中不可能有子串 $t$
但对于 $m=t+s+t$ ，可能有 $t的后缀+s+t的前缀=t$ 的情况出现，导致不满足题设条件//

那么下面感性证明一下两种方案至少有一种成立。
假设对于兼有 $0$ 和 $1$ 的 $t$ ，在 $s$ 全为 $0$ 的情况下出现：
$t的后缀+s+t的前缀=t$ 
那么构造 $s$ 全为 $1$ 即可，反之亦然

于是问题就转化为在 $s$ 全为 $1$ 或 $0$ 的方案中选择一种使得串$t$ 仅在开头和结尾各出现过一次
先使 $s$ 全为 $1$ 将 $m=t+s+t$ 去掉首尾一个字符，判断中间有无字串 $t$ 即可
归纳发现特殊情况也可以用这种方法构造

### 时间复杂度
数据范围小（ $n\le 1000,|t|\le 1000$ ），BF即可

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    string t;ll n;
    cin >> n >> t;
    string s;FORLL(i,1,n) s.push_back('1');
    string m=t+s+t;m.erase(m.begin());m.pop_back();
    if(m.find(t)!=string::npos) FORLL(i,1,n) cout << '0';
    else FORLL(i,1,n) cout << '1';
    cout << endl;
}
```