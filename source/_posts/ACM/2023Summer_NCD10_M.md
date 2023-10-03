---
title: 题解|M.Fair Equation-2023暑期牛客多校10
date: 2023/8/18
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 模拟
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: b01cdbfb
description: 题解|M.Fair Equation-2023暑期牛客多校10  Idx:2 模拟
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

# M.Fair Equation
**模拟**
## 题目大意
给定一个式子 $A+B=C$ ，其中正整数 $A,B,C\le 10^6$
问能否在 $A,B,C$ 其中一个数的某一位置（可以是开头和结尾）插入一个数字，使得等式成立；或原式自然成立。
若成立，输出成立的等式

## 解题思路
如题如题目所述，取出等式中的数字，逐位模拟即可。具体实现可参考代码

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

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