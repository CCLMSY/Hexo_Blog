---
title: 题解|2023暑期牛客多校02
date: 2023/7/21
updated: 2023/11/9
tags:
  - 2023暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: 9f517358
description: 题解|2023暑期牛客多校02
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

# D.The Game of Eating
**贪心**
## 题目大意
$n$ 个人聚餐， 在 $m$ 道菜品中选择 $k$ 道，且不能重复。从1号开始，每个人轮流选择一道菜
每个人对于每个菜品都有一个喜爱值，第 $i$ 个人对第 $j$ 个菜品的喜爱程度记为 $a_{i,j}$ 
假设每个人都想要使 自己对最终点的 $k$ 道菜的喜爱值总和 最大，求最终选定的菜单
## 解题思路
对于当前点菜的人，假设他最喜欢的菜还没有被点，但他仍然需要考虑后面是否有人选择这道菜。如果后面一定有人选择这道菜，他可以选择第二喜欢的菜品以使得自己的满意度最大化
基于这种情况分析，最后一个点菜的人无需考虑其他人的需求，只需要选择剩余菜品中最喜欢的那一个，即可使得自己的满意度最大化
假设所有菜品中他最喜欢的还没有被选取，那么就会选择这个菜品。反推到倒数第二个、第三个亦此

因此，只需要将点菜的顺序逆转，这样只要每个人选择剩余菜品中最喜欢的菜品即可，无需考虑其他人的需求。

### 时间复杂度
$k$ 次遍历菜品： $O(km)$
## 参考程序
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
int solve()
{
    ll m,n,k,t;
    cin >> n >> m >> k;
    vector<vector<pll>> v(n+1);
    vector<ll> re;
    vector<pll>::iterator it;
    vector<bool> chosen(m+1,false);
    FORLL(i,1,n){
        FORLL(j,1,m){
            cin >> t;
            v[i].emplace_back(t,j);
        }
        SORT(v[i]);
    }
    ll p=(k-1)%n+1;
    while(k--){
        it=v[p].end();--it;
        while(chosen[(*it).second]) it--;
        re.emplace_back((*it).second);
        chosen[(*it).second]=true;
        p--;
        if(p==0) p=n;
    }
    SORT(re);
    for(auto x:re){
        cout << x << ' ';
    }cout << endl;
    return 0;
}
```

***

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

***

# F.Link with Chess Game
**二分图博弈**
## 题目大意
在一行长度为 $n$ 的格子上有红绿蓝三种颜色的棋子，他们的坐标构成一个有序三元组 $(r,g,b)$ （坐标可以重复）

每次可以选择一颗棋子向左或向右移动一步，如果某次移动后的三元组 $(r_i,g_i,b_i)$ 在此前出现过，则执行移动的人失败

## 解题思路
### 赛时想法
假设只有一颗棋子的情况
选定一个方向，假设棋子当前距离这个方向端点的距离为 $x$ 。不难证明 $x$ 为奇数时胜利； $x$ 为偶数时失败
在这种情况下，只要当前位置与两端之一的距离为奇数即可获胜
基于这一结论，猜想三颗棋子的条件下，每颗棋子与左、右两端的距离之和 $suml,sumr$ 与必胜态之间的关系，和一颗棋子的情况相似
样例满足这一猜想
```cpp
int solve()
{
    ll n,a,b,c;
    cin >> n >> a >> b >> c;
    ll l,r;
    l=a+b+c-3;
    r=n*3-a-b-c;
    if(l&1||r&1) cout << "Alice" << endl;
    else cout << "Bob" << endl;
    return 0;
}
```
### 赛后补题
对于这类无向的博弈，可以考虑是否为二分图博弈
把三颗棋子看作是三个维度，显然所有状态组成的图是一个正方体，一定是二分图
在 $n$ 为偶数的情况下，最大匹配可以覆盖所有状态，即初始状态一定在某一对匹配上。这种情况下，先手一定落在匹配中的最后一个点，因此是必胜的
在 $n$ 为奇数的情况下，对于起始点在较小一部的状态点，一定在某一最大匹配上。这种情况下，先手一定落在匹配中的最后一个点，因此是必胜的
对于起始点在较大一部中的状态点的情况，一定存在一个最大匹配不包含起始点。因此在这个最大匹配上，后手一定落在匹配中的最后一个点，因此是必败的
赛时的思路虽然不严谨，但误打误撞结论和正解是一致的//

## 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
int solve()
{
    ll n,a,b,c;
    cin >> n >> a >> b >> c;
    if(n&1)
        if((a+b+c-1)&1) cout << "Alice" << endl;
        else cout << "Bob" << endl;
    else cout << "Alice" << endl;
    return 0;
}
```

***

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