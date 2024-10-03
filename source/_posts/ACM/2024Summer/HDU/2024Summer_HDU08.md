---
title: 题解|2024暑期杭电多校08
date: 2024/8/12
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU08
description: 题解|2024暑期杭电多校08
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（8）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A88%A3%A9&source=1&searchmode=source)

# (1004)HDU7520.cats 的重力拼图
## 题意
有一个 $n\times m$ 的方格阵列，物块初始位于 $(x,y),1\le x\le n,1\le y\le m$。
每次操作可以改变重力方向：向上、向下、向左、向右，物块会沿重力方向移动，直到碰到边界。
求任意操作下物块最多经过的格子数。

## 解题思路
有2种最贪心的操作序列：
1. 向左、向右、再沿边缘一周
2. 向上、向下、再沿边缘一周

特判 $n=1$ 或 $m=1$ 或初始在边缘的情况。

## 参考代码
```cpp
void solve()
{
    ll n,m,a,b;cin >> n >> m >> a >> b;
    if(n<=2||m<=2){ cout << n*m << endl; return ; }
    ll ans=2*(n+m-2);
    if(a==1||a==n){
        if(b==1||b==m) cout << ans << endl;
        else cout << ans+n-2 << endl;
    }else if(b==1||b==m) cout << ans+m-2 << endl;
    else cout << ans+max({0ll,n-2,m-2}) << endl;
}
```

# （1006）HDU7522.cats 的最小生成树
## 题意
给定一个有 $n$ 个节点，可能含重边的带权无向图， $m$ 条边按顺序给出，第 $i$ 条边的权值为 $i$。
每次删去当前图的最小生成树的所有边，直到图不连通。

求每条边是在第几次被删除去的。

## 解题思路
根据Kruskal算法思想，最小生成树加边是从小到大加入的。
开若干个并查集，遍历边，每次二分查找当前边最早可以加入第几个并查集。

## 参考代码
```cpp
void solve()
{
    ll n,m;cin >> n >> m;
    ll ub=m/(n-1);
    vector<DSU> dsu(ub+2,DSU(n));
    vector<ll> cnt(ub+2,0);
    vector<ll> ans(m+1,0);
    ll u,v;
    FORLL(i,1,m){
        cin >> u >> v;
        ll l=1,r=ub+1;
        while(l<r){
            ll mid=(l+r+1)/2;
            bool fl=(dsu[mid].find(u)==dsu[mid].find(v));
            if(fl) l=mid;
            else r=mid-1;
        }
        ll tar=0;
        FORLL(i,max(1ll,l-3),min(ub+1,l+3)){
            if(dsu[i].find(u)!=dsu[i].find(v)){
                tar=i;
                break;
            }
        }
        if(tar==ub+1) { ans[i]=-1; continue; }
        dsu[tar].merge(u,v);
        cnt[tar]++; ans[i]=tar;
    }
    FORLL(i,1,m){
        if(ans[i]!=-1&&cnt[ans[i]]==n-1) cout << ans[i];
        else cout << "-1";
        cout << Presentation(i,m);
    }
}
```

# (1007)HDU7523.cats 的 k-xor
## 题意
给定2个十进制整数 $a,b,c$ ， $a,b$ 进行 $k(k\ge 2)$ 进制不进位加法后的结果是 $c$。
求 $k$ 有多少种可能。

## 解题思路
不进位加法下，丢失的进位信息 $a+b-c$ 是 $k$ 的倍数。
枚举 $a+b-c$ 的因子作为 $k$ ，check是否满足条件。

## 参考代码
```cpp
bool check(ll a,ll b,ll c,ll k){
    ll dif=a+b-c;
    ll cur=1,nxt=k,s=0;
    while(a/cur||b/cur){
        ll ta=a/cur%k,tb=b/cur%k;
        s+=(ta+tb)/k*nxt;
        cur*=k; nxt*=k;
    }
    return s==dif;
}
void solve()
{
    ll a,b,c;cin >> a >> b >> c;
    ll dif=a+b-c;
    if(dif==0){
        cout << "-1" << endl;
        return ;
    }
    ll cnt=0,ub=sqrt(dif)+1;
    FORLL(i,2,ub) if(dif%i==0){
        if(i*i>dif) break;
        if(check(a,b,c,i)) cnt++;
        if(i*i!=dif&&check(a,b,c,dif/i)) cnt++;
    }
    if(check(a,b,c,dif)) cnt++;
    cout << cnt << endl;
}
```

# （1012）HDU7528.cats 的电脑中毒
## 题意
给定 $3$ 个长度为 $n$ 的二进制串 $a,b,c$ ， 表示病毒的初始位置。
每过一秒，病毒会感染相邻的所有二进制编码。（当且仅当两个二进制编码仅有一个位置不同时，这两个编码为相邻）
问所有的 $2^n$ 个二进制编码都被感染需要多少时间。

## 解题思路
考虑最后一秒被感染的二进制串，它的 距离三个初始位置的最小汉明距离 最大，找到这个串。
设这个串为 $s$ ，枚举每一位。若三个初始位置的这一位上，1的数量较多，则设为0；否则设为1。
然后进行微调，使得 $s$ 到三个初始位置的最小汉明距离 最大。

## 参考代码
```cpp
ll n;
ll dis(string s1,string s2){
    ll cnt=0;
    FORLL(i,0,n-1) cnt+=(s1[i]!=s2[i]);
    return cnt;
}
void solve()
{
    cin >> n;
    string s[3],ns(n,'0');
    FORLL(i,0,2) cin >> s[i];
    ll cnta=0;
    FORLL(i,0,n-1){
        int cnt1=0,t=0;
        FORLL(j,0,2) if(s[j][i]=='1') cnt1++;
        ns[i]=(cnt1>=2)?'0':'1';
    }
    ll dis0=dis(s[0],ns),dis1=dis(s[1],ns),dis2=dis(s[2],ns);
    while(dis0<dis1-1&&dis0<dis2-1){ dis0++; dis1--; dis2--; }
    while(dis1<dis0-1&&dis1<dis2-1){ dis1++; dis0--; dis2--; }
    while(dis2<dis0-1&&dis2<dis1-1){ dis2++; dis0--; dis1--; }
    cout << min({dis0,dis1,dis2}) << endl;
}
```