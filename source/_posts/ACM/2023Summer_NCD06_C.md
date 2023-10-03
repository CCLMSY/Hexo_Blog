---
title: 题解|C.idol!!-2023暑期牛客多校06
date: 2023/8/4
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
abbrlink: 40a05e08
description: 题解|C.idol!!-2023暑期牛客多校06 Idx:3 数学
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

# C.idol!!
**数学**
## 题目大意
正整数 $n$ 的双阶乘 $n!!$ 表示不超过 $n$ 且与 $n$ 有相同奇偶性的所有正整数乘积
求对于给定 $n$ ，$\prod\limits_{i=1}^n i!!$ 的后缀 $0$ 个数

## 解题思路
根据双阶乘的性质，可以得到： $(n-1)!!\times n!!=n!$
因此对于给定的 $n$ ，原式可化为：
$$\prod\limits_{i=1}^n i!!=\begin{cases}
    \prod\limits_{i=1}^\frac{n}{2} (2i)! &,n为偶数 \newline
    \prod\limits_{i=1}^\frac{n+1}{2} (2i-1)! &,n为奇数 
\end{cases}$$
显而易见的，阶乘中因子 $2$ 的个数一定多于因子 $5$ 的个数，因此题目等价于求上式中因子 $5$ 的个数//

考虑某单一阶乘 $n!$ 中所含因子 $5$ 的个数。
可以发现，每个 $5$ 的倍数项会提供 $1$ 个因子 $5$ ，共有 $\lfloor \dfrac{n}{5} \rfloor$ 项
除此之外每个 $25=5^2$ 的倍数项会额外提供一个因子 $5$ ，共有 $\lfloor \dfrac{n}{5^2} \rfloor$ 项
再除此之外每个 $125=5^3$ 的倍数项会额外提供一个因子 $5$ ，共有 $\lfloor \dfrac{n}{5^3} \rfloor$ 项……
因此对于单一阶乘 $n!$ ，其提供因子 $5$ 的数量 $cnt_5=\sum\limits_{i=1}^N \lfloor \dfrac{n}{5^i} \rfloor (5^N>n)$ 

接着考虑连乘积中因子 $5$ 个数的总和。
$$
ans=\begin{cases}
    \sum\limits_{i=1}^\frac{n}{2} \sum\limits_{j=1}^N \lfloor \dfrac{2i}{5^j} \rfloor=\sum\limits_{i=1}^N \sum\limits_{j=1}^\frac{n}{2} \lfloor \dfrac{2j}{5^i} \rfloor &,n为偶数 \newline
    \sum\limits_{i=1}^\frac{n+1}{2} \sum\limits_{j=1}^N \lfloor \dfrac{2i-1}{5^j} \rfloor=\sum\limits_{i=1}^N \sum\limits_{j=1}^\frac{n+1}{2} \lfloor \dfrac{2j-1}{5^i} \rfloor &,n为奇数 
\end{cases} \newline
$$

对于某一 $i$ ，发现不论 $n$ 的奇偶， $j=1$ 开始的每 $5^i$ 项之和构成公差为 $2\times5^i$ 的等差数列//
例：$i=1$ ，$n$ 为偶数且足够大时，$\lfloor \dfrac{2j}{5^i} \rfloor$ 的前 $15$ 项如下，其中每 $5$ 项之和构成公差为 $5\times 2$ 的等差数列： $0,0,1,1,2||2,2,3,3,4||4,4,5,5,6……$

经计算，对于某一 $i$ ，等差数列的首项为
$$
a_1=\begin{cases}
    \lfloor \dfrac{5^i}{2} \rfloor+2 &,n为偶数 \newline
    \lfloor \dfrac{5^i}{2} \rfloor+1 &,n为奇数
\end{cases}
$$

完整的段用等差数列求和，非完整的段手算一下//
​
若此前完整段的数量记为 $m$ ，则非完整段：
前 $\lfloor \dfrac{5^i}{2} \rfloor$ 项的值为 $2m$ ，
第 $\lfloor \dfrac{5^i}{2} \rfloor+1$ 至 $2\times\lfloor \dfrac{5^i}{2} \rfloor $ 项的值为 $2m+1$（手搓一下就知道了）

求和即可

令 $N=\lfloor \log_5n \rfloor+1$ ，对 $i\in[1,N]$ 遍历求和得到答案

由于答案数据极其庞大，超出了C++ %lld(64bits)的范围，因此需要使用更高位数的整数类型（如int128）//或者直接转战Python

### 时间复杂度
$O(\log n)$

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```python
import math
# while 1:
n=int(input())
N=int(math.log(n,5)+1)
re=0
if n%2==0 :
    for i in range(1,N+1) :
        #print("i="+str(i))
        a1=(5**i)//2+2 #首项
        #print("a1="+str(a1))
        d=(5**i)*2 #公差
        #print("d="+str(d))
        m=(n//2)//(5**i) #完整段数
        #print("m="+str(m))
        re+=(2*a1+(m-1)*d)*m//2 #完整段等差数列求和
        #print("re1:" + str(re))
        re+=(n//2-m*(5**i))*2*m #最后一段余项求和
        #print("re2:" + str(re))
        #print("pl1=" + str((n//2-m*(5**i))*2*m))
        if n//2-m*(5**i)>(5**i)//2 :
            re+=n//2-m*(5**i)-(5**i)//2
            #print("pl2=" + str(n//2-m*(5**i)-(5**i)//2))
                

if n%2 :
    for i in range(1,N+1) :
        #print("i="+str(i))
        a1=(5**i)//2+1 #首项
        #print("a1="+str(a1))
        d=(5**i)*2 #公差
        #print("d="+str(d))
        m=((n+1)//2)//(5**i) #完整段数
        #print("m="+str(m))
        re+=(2*a1+(m-1)*d)*m//2 #完整段等差数列求和
        #print("re1:" + str(re))
        re+=((n+1)//2-m*(5**i))*2*m #最后一段余项求和
        #print("re2:" + str(re)) 
        #print("pl1=" + str(((n+1)//2-m*(5**i))*2*m))
        if (n+1)//2-m*(5**i)>(5**i)//2 :
            re+=(n+1)//2-m*(5**i)-(5**i)//2
            #print("pl2=" + str((n+1)//2-m*(5**i)-(5**i)//2))
    
print(re)
```