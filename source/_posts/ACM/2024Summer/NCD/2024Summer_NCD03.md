---
title: 题解|2024暑期牛客多校03
date: 2024/7/23
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD03
description: 题解|2024暑期牛客多校03
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

比赛链接：[2024牛客暑期多校训练营3](https://ac.nowcoder.com/acm/contest/81598)

# A.Bridging the Gap 2
## 题意
$n$个人过河，第 $i$ 个人初始有 $h_i$ 点体力。
由于船的限制，每次过河（或返回）至少需要乘坐 $l$ 人（来划船），至多可以乘坐 $r$ 人，**每个**乘船的人都会消耗 $1$ 点体力。体力为 $0$ 的人无法乘船。
求对于给定的条件，是否能够使所有人过河。

## 解题思路
假设初始所有人在左岸，考虑一种贪心模拟的做法：
- 从在左岸的所有人中选取 $l$ 个体力最大的人划船，带 $r-l$ 个体力最小的人去右岸。
- 从在右岸的所有人中选取 $l$ 个体力最大的人划船返回左岸。
- 重复以上步骤，直到所有人都到达右岸，或者无法继续。

这个过程中，除去最后一次划到右岸的 $r$ 个人，每次能运输的人数为 $r-l$ 。
最低**往返**的次数为 $turn = \lceil \dfrac{n-r}{r-l}\rceil$ ，且最优，因为往返越少对体力的要求越低。

对于个人，除自己前往右岸的1点体力，多余的体力可以用于划船带人，往返一次需要2点体力。
因此第 $i$ 个人能够参与的往返次数为 $\lfloor \dfrac{h_i-1}{2}\rfloor$ 。
由于只存在 $turn$ 次往返，因此第 $i$ 个人能够参与的往返次数为 $\min(\lfloor \dfrac{h_i-1}{2}\rfloor,turn)$ 。

计算所有人能够参与的往返次数之和，如果大于等于 $turn*l$ ，则按照上述贪心模拟的方法，可以使所有人过河。

## 参考程序
> 程序是副机长根据解题思路写的，居然A了()
```cpp
void solve()
{
    ll n,l,r; cin >> n >> l >> r;
    vector<ll> v(n);
    for(auto&x:v) cin >> x;
    ll turn = (n-r)/(r-l) + ((n-r)%(r-l)!=0);
    ll sum = 0;
    for(auto x:v) sum += min((x-1)/2,turn);
    cout << (sum>=turn*l?"Yes":"No") << endl;
}
```

# B.Crash Test
## 题意
初始距离墙壁的距离为 $d$ 。
每次前进有 $n$ 种长度可以选择：$h_1,h_2,\cdots,h_n$。每次前进的长度可以是任意一种长度。
如果选择的长度 $h_i$ 大于当前与墙壁的距离 $d'$ ，将会退后多余的距离，即新的距离为 $h_i - d'$ 。
求在任意次（包括0次）前进后，与墙壁的最小距离。

## 解题思路
[裴蜀定理](https://baike.baidu.com/item/%E8%A3%B4%E8%9C%80%E5%AE%9A%E7%90%86/5186593)：对于非0整数 $a,b$ ，对任意整数 $x,y$ 有 $gcd(a,b)|ax+by$ 成立，即 $gcd(a,b)$ 是所有 $a,b$ 的线性组合中，绝对值最小的非0整数。

裴蜀定理扩展到多整数的情况仍然成立。

因此计算出 $g=\gcd\limits_{i=1}^n(h_i)$ ，$g$ 的意义是通过对 $h_i$ 的某种线性组合，能够得到的最小前进距离。

然后每一步视为走 $g$ ，以此求得不撞墙答案 $d\%g$ 与撞墙答案 $g-d\%g$ ，取较小值即可。

## 参考程序
```cpp
void solve()
{
    ll n,d; cin >> n >> d;
    create_vec(v,n);
    ll g = v[0];
    for(auto x:v) g = __gcd(g,x);
    cout << min(d%g,g-d%g) << endl;
}
```

# D.Dominoes!
//TODO

# J.Rigged Games
//TODO

# L.Sudoku and Minesweeper
## 题意
经典数独在 $9\times 9$ 大小的棋盘格内进行，每一行、每一列、$9$ 个 $3\times 3$ 的小方块内，数字 $1-9$ 恰好出现一次。

扫雷是一款在棋盘格内进行的游戏，中心数字表示周围 $8$ 格包含地雷的数量。

现给定一个 $9\times 9$ 数字矩阵表示一个已经完成的合法经典数独，可以将里面的数字替换成地雷，但必须保留至少 $1$ 个数字，求一个合法的扫雷游戏布局。

## 解题思路
除了边缘之外，中间 $7\times 7$ 范围内必然出现数字 $8$ 。
这是一个特殊的数字，只需要把它保留，其余所有数字全部替换成地雷，就是一个合法的扫雷游戏布局。

## 参考程序
```cpp
void solve()
{
    vector<string> vs(9);
    for(auto&s:vs) cin >> s;
    int fl=0,i8,i8;
    FORLL(i,1,7){
        FORLL(j,1,7){
            if(vs[i][j]=='8'){
                i8=i; i8=j; fl=1; break;
            }
        } if(fl) break;
    }
    FORLL(i,0,8){
        FORLL(j,0,8){
            if(i==i8&&j==i8) cout << '8';
            else cout << '*';
        }
        cout << endl;
    }
}
```