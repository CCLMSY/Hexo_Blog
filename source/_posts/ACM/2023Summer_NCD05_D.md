---
title: 题解|D.Cirno's Perfect Equation Class-2023暑期牛客多校05
date: 2023/7/31
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 思维
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: d16d9684
description: 题解|D.Cirno's Perfect Equation Class-2023暑期牛客多校05 Idx:2 思维
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

# D.Cirno's Perfect Equation Class
**签到**
## 题目大意
给定三个整数 $k,c,n$ 
求满足以下条件的有序对 $(a,b)$ 的个数： $ka+b=c$ ， $b|c$ ， $gcd(a,b)>n$
## 解题思路
注意到第二个约束条件： $b$ 是 $c$ 的因子
因此对 $b$ 枚举，判断计数即可
### 时间复杂度
$O(qc^\frac{1}{2})$
## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
	ll k,c,n,a,tb;
	cin >> k >> c >> n;
	ll r=floor(sqrt(c)),cnt=0;
	for(ll b=1;b*b<=c;b++){
		if(c%b==0&&(c-b)%k==0){
			a=(c-b)/k;
			if(a&&b&&gcd(a,b)>=n) cnt++;
		}
		tb=c/b;
		if(tb!=b&&c%tb==0&&(c-tb)%k==0){
			a=(c-tb)/k;
			if(a&&tb&&gcd(a,tb)>=n) cnt++;
		}
	}cout << cnt << endl;
}
```