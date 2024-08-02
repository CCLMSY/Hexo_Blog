---
title: 题解|2023暑期杭电多校03
date: 2023/7/25
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_HDU03
description: 题解|2023暑期杭电多校03
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

# 1011.8-bit Zoom
**不那么签到的签到题、模拟题**
## 题意
给定一个 $n\times n$ 大小的字符矩阵表示一张图片，每种字符代表一种颜色；并给定 $Z$ 代表缩放倍率
满足以下条件即可缩放：
1. 缩放后的边长 $\dfrac{nZ}{100}$ 是一个整数
2. 每个像素可以被唯一确定（如果缩放后的像素被从超过两种颜色覆盖，则无法确定颜色）

如果可以进行缩放，则输出缩放后的图片，否则输出“error”

## 解题思路
模拟缩放过程

如果不满足上述条件 $1$ 则一定无法缩放

条件 $2$ 需要根据图片实际情况判断是否满足
记这张图片被放大了 $x=\dfrac{a}{b}$ 倍，假设此处 $a,b$ 互质，则等同于先将图片缩小 $b$ 倍，再放大 $a$ 倍
如果缩小的过程不会有像素无法被确定，则缩小后一定是可以放大整数倍的
因此只需检查每个边长为 $b$ 的像素区块（它们将被缩小至 $1$ 像素），如果有任意一块混色，则无法压缩
压缩成功后，边长放大至 $a$ 倍即可

根据题意，记缩放后的图片尺寸为 $sz$
易得此处 $a=sz/gcd(n,sz)$ ， $b=n/gcd(n,sz)$ 

## 参考代码
```cpp
void solve()
{
    ll n,Z,t;
    cin >> n >> Z;
    string s[55];
    FORLL(i,1,n) cin >> s[i];
    if((n*Z)%100) {cout << "error" << endl;return;}
    ll sz=(n*Z)/100;//缩放后的图片边长
    char re[55][55]={0};
    t=gcd(sz,n);
    if(n==t){
        FORLL(i,1,n){
            FORLL(a,1,sz/t){
                FORLL(j,0,n-1){
                    FORLL(b,1,sz/t){
                        cout << s[i][j];
                    }
                }
                cout << endl;
            }
        }return;
    }
    for(ll i=1;i<=t;i++){
        for(ll j=0;j<t;j++){
            char ch=s[(i-1)*(n/t)+1][j*(n/t)];
            FORLL(a,0,n/t-1){
                FORLL(b,0,n/t-1){
                    if(s[(i-1)*(n/t)+1+a][j*(n/t)+b]!=ch){
                        cout << "error" << endl;
                        return ;
                    }
                }
            }
            re[i-1][j]=ch;
        }
    }
    FORLL(i,0,t-1){
        FORLL(a,1,sz/t){
            FORLL(j,0,t-1){
                FORLL(b,1,sz/t) cout << re[i][j];
            }
            cout << endl;
        }
    }
}
```