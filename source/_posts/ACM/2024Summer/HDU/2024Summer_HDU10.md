---
title: 题解|2024暑期杭电多校10
date: 2024/8/18
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU10
description: 题解|2024暑期杭电多校10
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（10）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A810%A3%A9&source=1&searchmode=source)

# (1008)HDU7548.SunBian
## 题意
有排成环形的 $n$ 个横着的笋，Alice 和 Bob 轮流执行如下操作，Alice 先手：
- 选择 [1,k] 个连续的横着的笋，把它们变成竖着的
 
不能操作者输。

给定 $n,k$ ，求谁会赢。

## 解题思路
- 当 $k=1$ 时，根据奇偶性判断赢家
- 当 $k\ge n$ 时，先手直接将笋全部竖置，必胜
- 其余情况下，后手每次都可以尽可能保证剩余区域数为偶数，最终必胜

## 参考代码
```cpp
void solve()
{
    ll n,k;cin >> n >> k;
    if(n<=k) cout << 'A';
    else if(k==1) cout << (n%2?'A':'B');
    else cout << 'B';
}
```

# (1009)HDU7549.不基本子串结构
## 题意
给定2个字符串 $a,b$ ，找到一个最小长度的字符串 $c$ ，使得 $a$ 和 $b$ 在 $c$ 中出现的次数相等且不为0，输出最小长度。

## 解题思路
分类讨论，不妨假设 $len(a)\le len(b)$
- 若 $a$ 在 $b$ 中出现的次数大于 $1$ ，则不存在满足条件的 $c$
- 若 $a$ 在 $b$ 中出现的次数为 $1$ ，则 $c=b$ ，输出 $len(b)$
- 若 $a$ 在 $b$ 中没有出现：
  - 记 $l_1$ 为 最大满足 $a[0:l]=b[len(b)-l:len(b)]$ 的 $l$
  - 记 $l_2$ 为 最大满足 $a[len(a)-l:len(a)]=b[0:l]$ 的 $l$
  - 答案为 $len(a)+len(b)-max(l_1,l_2)$

可以用字符串哈希进行检查和计数。

## 参考代码
```cpp
void solve()
{
    string s1,s2;
    cin >> s1 >> s2;
    if(s1.length()<s2.length()) swap(s1,s2);
    strHash h1(s1),h2(s2);
    ll n=s1.length(),m=s2.length();
    ll cnt=h1.count(h2);
    if(cnt>1){
        cout << "-1" << endl;
        return;
    }
    if(cnt==1){
        cout << n << endl;
        return;
    }
    ll ans=m+n;
    FORLL_rev(len,m-1,1){
        if(h1.findz(n-len+1,n)==h2.findz(1,len)){
            chmin(ans,n+m-len);
            break;
        }
    }
    FORLL_rev(len,m-1,1){
        if(h1.findf(1,len)==h2.findf(m-len+1,m)){
            chmin(ans,n+m-len);
            break;
        }
    }
    cout << ans << endl;
}
```

# （1011）HDU7551.NOI2024
## 题意
$m$ 名选手进行 $n$ 场比赛，排名定义为分数严格大于你的人数+1。
第 $i$ 场比赛的分数上限为 $b_i$ ，你的排名为 $a_i$ 。
最终按照每场比赛的总分排名，前 $k$ 名选手将获得金牌。
问在给定条件下不管怎么比赛，是否一定能获得金牌。

## 解题思路
用最坏情况考虑：你始终为 $0$ 分，在你前面的选手都有分数。
最终最坏排名为 $\min(\sum_{i=1}^{n}a_i,m)$ 。

## 参考代码
```cpp
void solve()
{
    ll n,m,k;cin >> n >> m >> k;
    create_vec(a,n);
    create_vec(b,n);
    ll cnt=0;
    FORLL(i,0,n-1) cnt+=max(0ll,a[i]-1);
    chmin(cnt,m-1);
    if(cnt>=k) cout << "NO" << endl;
    else cout << "YES" << endl;
}
```