---
title: 题解|I.Link with Gomoku-2023暑期牛客多校02
date: 2023/7/21
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 构造
  - 签到
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: b3977898
description: 题解|I.Link with Gomoku-2023暑期牛客多校02 Idx:1 构造
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

# I.Link with Gomoku
**构造**
## 题目大意
>“你赢得越多，你的自信就越强。你的自信越强，你就越傲慢。你越傲慢，就越容易犯错误。你犯的错误越多，你就输的越多。所以你赢的越多，你输的就越多。”

在一个 $n\times m$ 大小的棋盘上下五子棋，构造一个平局的棋盘局面。
## 解题思路
构造的方法并不难想，这里给出如下两种：
1. 题目样例所示的：
   xxxxooooxxxxoooo
   ooooxxxxooooxxxx
   xxxxooooxxxxoooo
   ooooxxxxooooxxxx
2. 我赛时所写的：
   ooxooxooxoo
   xxoxxoxxoxx
   ooxooxooxoo
   xxoxxoxxoxx

易错点在于：只关注了平局的条件，没有在意黑白子之间的数量关系

如果是偶数行，按上面方案输出；
如果是奇数行，最后一行按xoxoxox的顺序输出即可

## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
int solve()
{
    ll m,n;
    cin >> n >> m;
    int flag=n&1;
    if(flag) n=n-1;
    FORLL(i,1,n){
        if(i&1){
            FORLL(j,1,m){
                if(j%3) cout << 'o';
                else cout << 'x';
            }
        }
        else{
            FORLL(j,1,m){
                if(j%3) cout << 'x';
                else cout << 'o';
            }
        }
        cout << endl;
    }
    if(flag) FORLL(j,1,m){
        if(j&1) cout << 'x';
        else cout << 'o';
    }
    if(flag) cout << endl;
    return 0;
}
```