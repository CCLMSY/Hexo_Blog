---
title: 题解|H.Until the Blue Moon Rises-2023暑期牛客多校03
date: 2023/7/24
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
abbrlink: 2e626fae
description: 题解|H.Until the Blue Moon Rises-2023暑期牛客多校03 Idx:2 哥德巴赫猜想
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

# H.Until the Blue Moon Rises
**数学题**
## 题目大意
对于给定的 $n$ 个数，每次操作可以使一个数 $-1$ ，另一个数 $+1$ 
可以操作任意次，问是否可以使这些数全部成为质数
### 前置知识点
[哥德巴赫猜想](https://baike.baidu.com/item/%E5%93%A5%E5%BE%B7%E5%B7%B4%E8%B5%AB%E7%8C%9C%E6%83%B3/72364)：任一大于5的整数都可写成三个质数之和，任一大于2的偶数都可写成两个素数之和
## 解题思路
题设操作任意次的效果即：把所有数的和 $sum$ 分成 $n$ 个数
问是否有一种分法使得这 $n$ 个数都是质数

1. $n$ 个数都是质数时，最小情况是所有的数全为 $2$ 
   因此，当 $sum<2\times n$ 时一定不可以构成
2. $n=1$ 
   只有一个数，直接判断它是否为质数即可
3. $n=2$ 
   如果它们的和是偶数，根据哥德巴赫猜想一定可以构成；
   如果它们的和是奇数，分成两个数一定是一奇一偶，众嗦粥汁，偶数中只有2是质数，因此判断 $n-2$ 是否为质数即可
4. $n=3$ 
   由于 $sum<2\times n=6$ 的情况已经在 $1$ 中筛去，故根据哥德巴赫猜想一定可以构成
5. $n>3$
   每次分出 $2$ 直到剩余 $3$ 个数，利用上一条可知一定可以构成

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
bool checkprime(ll n){
    if(n<2) return false;
    if(n==2||n==3) return true;
    ll t=sqrt(n)+1;
    FORLL(i,2,t) if((n/i)*i==n){
        return false;
    }
    return true;
}
#define N 100005
void solve()
{
    ll t,sum=0;
    ll n;
    cin >> n;
    FORLL(i,1,n){
        cin >> t;
        sum+=t;
    }
    if(sum<2*n) {cout << NO ;return;}
    if(n==1){
        if(checkprime(sum))
        {
            cout << YES;
            return;
        }
        cout << NO ;
        return;
    }
    if(n==2){
        if(sum&1){
            if(checkprime(sum-2)) {cout << YES ;return;}
            cout << NO;
            return;
        }
        cout << YES;
        return;
    }
    cout << YES;
}
```