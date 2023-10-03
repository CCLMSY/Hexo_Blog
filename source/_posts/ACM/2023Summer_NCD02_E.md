---
title: 题解|E.Square-2023暑期牛客多校02
date: 2023/7/21
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 数学
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 8a346a5a 
description: 题解|E.Square-2023暑期牛客多校02 Idx:2 数学
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

# E.Square
**数学题**
## 题目大意
对于给定的整数 $x\le 10^9$ ，找到一个 $y\le 10^9$ ，使得存在一个整数 $k$，使得 $x$ 是 $y^2$ 的前 $k$ 位
如果不存在这样的 $y$ ，输出 $-1$

## 解题思路
题目给定了一个定义式：$\lfloor \dfrac{y^2}{10^k} \rfloor = x$
可以将其化解为不等式：
$$\begin{align}
    原式
    &\Rightarrow x \le \dfrac{y^2}{10^k} \lt x+1 \newline
    &\Rightarrow 10^k x \le y^2 \lt 10^k (x+1) \newline
    &\Rightarrow \sqrt{10^k x} \le y \lt \sqrt{10^k(x+1)}
\end{align}
$$
其中 $x$ 是确定的，只需遍历 $k$ 对应的 $y=sqrt(10^k x)+1$ ，判断即可

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
void solve()
{
    ll x;
    cin >> x;
    ll t=sqrt(x);
    if(t*t==x){
        cout << t << endl;
        return ;
    }
    ll p=1;
    while(1){
        p*=10;//表示10^k
        t=sqrt(p*x);
        if(t*t<t*n) t++;
        if(t>1e9){
            cout << "-1" << endl;
            return ;
        }
        if((t*t)/p==x){
            cout << t << endl;
            return ;
        }
    }
}
```