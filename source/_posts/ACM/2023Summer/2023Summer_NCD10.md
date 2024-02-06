---
title: 题解|2023暑期牛客多校10
date: 2023/8/18
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: f6ff419e
description: 题解|2023暑期牛客多校10
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

# K.First Last
**签到题**
## 题目大意
$n$ 个人参加 $m$ 场比赛，每场比赛中获得名次得概率均等
问针对某一人，他在所有场次比赛中都获得第一或倒数第一的概率

## 解题思路
如果人数 $n>1$ ，每场比赛的概率是 $p=\dfrac{2}{n}$ ；人数为 $1$ 时 $p=1$

输出 $p^m$ 即可


## 参考代码
```cpp
void solve()
{
    ll m,n;cin >> n >> m;
    double p=2./n,re=1;
    if(n==1) p=1;
    FORLL(i,1,m) re*=p;
    print_float(re,15);cout << endl;
}
```

***

# M.Fair Equation
**模拟**
## 题目大意
给定一个式子 $A+B=C$ ，其中正整数 $A,B,C\le 10^6$
问能否在 $A,B,C$ 其中一个数的某一位置（可以是开头和结尾）插入一个数字，使得等式成立；或原式自然成立。
若成立，输出成立的等式

## 解题思路
如题如题目所述，取出等式中的数字，逐位模拟即可。具体实现可参考代码

## 参考代码
```cpp
void print_equal(){
    cout << YES;
    cout << num[0] << " + " << num[1] << " = " << num[2] << endl;
}
void solve()
{
    string s;getline(cin,s);
    get_nums(s);ll t;//分离出数字
    if(num[0]+num[1]==num[2]) {print_equal();return ;}//原式成立
    FORLL(i,0,9){
        FORLL(k,0,numlen[0]){
            t=(num[0]/pow_10[k])*pow_10[k]*10+i*pow_10[k]+num[0]%pow_10[k];
            if(t+num[1]==num[2]) {num[0]=t;print_equal();return ;}
        }//在A的第k位插入i，判断是否符合条件，下面同理
        FORLL(k,0,numlen[1]){
            t=(num[1]/pow_10[k])*pow_10[k]*10+i*pow_10[k]+num[1]%pow_10[k];
            if(num[0]+t==num[2]) {num[1]=t;print_equal();return ;}
        }
        FORLL(k,0,numlen[0]){
            t=(num[2]/pow_10[k])*pow_10[k]*10+i*pow_10[k]+num[2]%pow_10[k];
            if(num[0]+num[1]==t) {num[2]=t;print_equal();return ;}
        }
    }cout << NO;
}
```