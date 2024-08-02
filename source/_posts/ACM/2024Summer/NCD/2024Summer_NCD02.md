---
title: 题解|2024暑期牛客多校02
date: 2024/7/18
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD02
description: 题解|2024暑期牛客多校02
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

比赛链接：[2024牛客暑期多校训练营2](https://ac.nowcoder.com/acm/contest/81597)

# C.Red Walking on Grid

## 题意
一个2行 $n$ 列的网格上，有些格子是红色的，有些格子是白色的。
你可以最初选择一个红色的格子，然后每一步都可以选择上下左右相邻的红色格子。离开一个格子时，这个格子立即变成白色。问最多可以走多少步。
最初没有红色的格子，输出0。

## 解题思路
总体思路就是找连通块，在每个连通块内部找到能走最多格的路径，然后所有连通块的答案取最大值。
由于无法走回头路，形如以下样式的连通块将无法走遍全部红色格子：

![image](/images/ACM/2024Summer_NCD02_C.png)

先从头到尾扫一遍，有以上2种形式的连通块，将打×的部分预先涂成白色。这样就能保证每个红色格子都会被经过，计算到答案中。

## 参考程序
```cpp
array<string, 2> s;
ll n,ans=1;
ll getstat(ll col){
    if(s[0][col]=='W'&&s[1][col]=='W') return 0;
    if(s[0][col]=='W'&&s[1][col]=='R') return 1;
    if(s[0][col]=='R'&&s[1][col]=='W') return 2;
    if(s[0][col]=='R'&&s[1][col]=='R') return 3;
    return -1;
}
ll getcnt(ll col){
    if(s[0][col]=='W'&&s[1][col]=='W') return 0;
    if(s[0][col]=='W'&&s[1][col]=='R') return 1;
    if(s[0][col]=='R'&&s[1][col]=='W') return 1;
    if(s[0][col]=='R'&&s[1][col]=='R') return 2;
    return -1;
}
void preprocess(){
    ll cnt=0,pre=0,cur;
    if(getcnt(0)==1) pre=getstat(0);
    FORLL(i,1,n-1){
        cur = getcnt(i);
        if(cur==1){
            cur=getstat(i);
            if(pre&&cur&&cnt){
                if((cnt%2==0)&&((pre&cur)==0)) s[cur-1][i-1]='W';
                if((cnt%2)&&((pre&cur))) s[cur-1][i-1]='W';
            }
            pre=cur;
            cnt=0;
        }else if(cur==2){
            cnt++;
        }else{
            cnt=0; pre=0;
        }
    }
}
void getans(){
    ll cnt=getcnt(0),pre=getstat(0),cur;
    FORLL(i,1,n-1){
        cur = getstat(i);
        if(cur&pre){
            cnt+=getcnt(i);
        }else{
            chmax(ans,cnt);
            cnt=getcnt(i);
        }pre=cur;
    }
    chmax(ans,cnt);
}
void solve()
{
    cin >> n;
    cin >> s[0] >> s[1];
    ans=1;
    preprocess();
    getans();
    cout << ans-1 << endl;
}
```

# E.GCD VS XOR

## 题意
给定一个正整数 $x$ ，找到一个严格小于 $x$ 的正整数 $y$ ，使得 $gcd(x,y)=x\oplus y$ ，其中 $\oplus$ 表示按位异或。

## 解题思路
首先观察到，对于一个正整数 $x$ ，$lowbit(x)$ 一定是 $x$ 的因子。
那么令 $y = x - lowbit(x)$ ，则 $gcd(x,y)=lowbit(x)$ ，$x\oplus y = lowbit(x)$ ，满足题意。
> 有时候灵感就来源于一瞬间

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    ll ans=n-lowbit(n);
    cout << (ans?ans:-1) << endl;
}
```

# H.Instructions Substring

## 题意
你初始位于原点 $(0,0)$ 。
给定一串动作指令，包含`W`、`A`、`S`、`D`，分别表示向上（$y+=1$）、向左（$x-=1$）、向下（$y-=1$）、向右（$x+=1$）。
你需要选择一个连续的子串，使得执行这个子串的指令后，你能经过给定得一点 $(x,y)$ 。
计算符合条件的子串的个数。

## 解题思路
假设选择了子串 $s_{ij}$ ，恰好能到达 $(x,y)$ ，那么 $s_j$ 之后任意增加指令都已经满足了“经过 $(x,y)$”的条件。

最暴力的做法就是枚举开头指令 $i$ ，枚举恰好到达 $(x,y)$ 的最小结尾指令 $j$ ，以位置 $i$ 开头的方案数就是 $n-j+1$ 。时间复杂度 $O(n^2)$ ，包TLE的，需要优化搜索过程。

我们可以先从头到尾执行（即做一个前缀和），每一步都记录到达当前位置 $(x_i,y_i)$ 时执行的指令编号 $i$ 。
再重新从头到尾执行一边，对于每一步到达的位置 $(x_i,y_i)$ ，将它视作起点（即假设之前的步骤都没有执行），那么需要经过的点变为 $(x_i+x,y_i+y)$ 。二分找到在 $i$ 之后到达过 $(x_i+x,y_i+y)$ 的最小指令编号 $j$ ，以位置 $i$ 开头的方案数就是 $n-j+1$ 。

$x=0,y=0$ 的情况比较特殊，特判一下即可。

## 参考程序
```cpp
//                W  S  D  A
const int dx[] = {0, 0, 1, -1};
const int dy[] = {1, -1, 0, 0};
ll conv(char c){
    if(c=='W') return 0;
    if(c=='S') return 1;
    if(c=='D') return 2;
    if(c=='A') return 3;
    return -1;
}
void solve()
{
    ll n,x,y; cin >> n >> x >> y;
    string s; cin >> s;
    if(x==0&&y==0){
        cout << n*(n+1)/2 << endl;
        return ;
    }
    map<pll,vector<ll>> mp;
    ll cx,cy,dir; cx=cy=0;
    mp[{cx,cy}].emplace_back(0);
    for(ll i=0;i<n;i++){
        char c = s[i];
        dir = conv(c);
        cx += dx[dir];
        cy += dy[dir];
        mp[{cx,cy}].emplace_back(i);
    }
    
    ll ans = 0;
    cx=cy=0;
    for(ll i=0;i<n;i++){
        auto v = mp[{cx+x,cy+y}];
        auto it = lower_bound(ALL(v),i);
        if(it!=v.end()) ans+=n-(*it);
        char c = s[i];
        dir = conv(c);
        cx += dx[dir];
        cy += dy[dir];
    }
    auto v = mp[{cx+x,cy+y}];
    auto it = lower_bound(ALL(v),n);
    if(it!=v.end()) ans+=n-(*it);

    cout << ans << endl;
}
```