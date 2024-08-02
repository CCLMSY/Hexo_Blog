---
title: 题解|2024暑期牛客多校04
date: 2024/7/25
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD04
description: 题解|2024暑期牛客多校04
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

比赛链接：[2024牛客暑期多校训练营4](https://ac.nowcoder.com/acm/contest/81599)

# A.LCT
## 题意
给定一棵有根树，问按顺序给定的前 $i$ 条边组成的森林中，以 $c_i$ 为根的树的深度。

## 解题思路
按步骤生成森林的过程，与并查集合并的过程一致。
因此用带权并查集，维护每个点的深度和答案，利用路径压缩降低时间复杂度。

## 参考程序
```cpp
struct DSU{
    vector<ll> parents, size, dep, ans;
    explicit DSU(ll n) : parents(n + 1), size(n + 1, 1), dep(n + 1, 0), ans(n + 1, 0) { iota(parents.begin(), parents.end(), 0); }
    ll find(ll x) {
        if(x == parents[x]) return x;
        ll px = find(parents[x]);
        dep[x]+=dep[parents[x]]; 
        return parents[x]=px;
    }
    void merge(ll a, ll b)
    { // merge b into a
        ll pa = find(a);
        parents[b] = a;
        dep[b] = 1;
        find(b);
        chmax(ans[pa],ans[b]+dep[b]);
    }
    ll query(ll x) {return ans[x];}
};

void solve()
{
    ll n;cin >> n;
    DSU dsu(n);
    ll a,b,q;
    FORLL(i,1,n-1){
        cin >> a >> b;
        dsu.merge(a,b);
        cin >> q;
        cout << dsu.query(q) << Presentation(i,n-1);
    }
}
```

# C.Sort4
## 题意
给定一个长度为 $n$ 的排列 $p$ （即序列中 $1$ ~ $n$ 的每个数恰好出现一次）。
每次操作可以选择4个元素，并任意交换它们的位置。
求使得排列变为升序的最少操作次数。

## 解题思路
把排列看作由 $i\rightarrow p_i$（下标从1开始） 构成的图，这个图中有若干个环，表示这个环中的元素可以通过交换回到原来的位置。

长度为 $3,4$ 的环，可以通过一次操作还原；
长度为 $2$ 的环，可以 $2$ 个环一组通过一次操作还原；
长度大于 $4$ 的环，每次操作可以让 $3$ 个元素回到原来的位置，使得环的长度减少 $3$，直到环的长度小于等于 $4$。

根据这个原则，计算最终答案。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    vector<ll> v(n+1);
    FORLL(i,1,n) cin >> v[i];
    vector<int> vis(n+1,0);
    ll ans=0,of=0; //of:最终长度为2的环的数量
    map<ll,ll> mp;
    FORLL(i,1,n){
        if(vis[v[i]]) continue;
        ll cnt=1,cur=v[i];
        vis[i]=1;
        while(cur!=i){
            vis[cur]=1;
            cur = v[cur];
            cnt++;
        }
        if(cnt==1) continue;
        if(cnt%3==1) ans+=(cnt-1)/3; //最终长度为4的环
        else if(cnt%3==2){ //最终长度为2的环
            ans+=(cnt-2)/3;
            of++;
        }else ans+=cnt/3; //最终长度为3的环
    }
    cout << ans+(of+1)/2 << endl;
}
```

# H.Yet Another Origami Problem
## 题意
给定一个长度为 $n$ 的序列 $a$，每次可以选择一个元素 $a_i$，执行以下操作之一：
1. 所有原来比 $a_i$ 小的数 $a_j$ ： $a_j=a_i+2(a_i-a_j)$ ，即原来 $a_j$ 比 $a_i$ 小多少，现在就比 $a_i$ 大多少。
2. 所有原来比 $a_i$ 大的数 $a_j$ ： $a_j=a_i-2(a_j+a_i)$ ，即原来 $a_j$ 比 $a_i$ 大多少，现在就比 $a_i$ 小多少。

问任意次操作后，序列 $a$ 中最大元素和最小元素之差 $max(a)-min(a)$ 的最小值。

## 解题思路
每次操作：
1. 对序列 $a$ 排序去重，求出差分数组 $d$ 。
2. 选定次小元素 $a_2$ ，执行操作 $1$ ，使得最小元素 $a_1 = a_2 - d_1$ 变成 $a_1' = a_2 + d_1$ 。

假设重新排序去重后，新的 $a_1'$ 相邻的两项为 $a_j,a_{j+1}$ ，步骤1中这两项的差分是 $d_j=a_{j+1}-a_j$ 。
那么当 $a_i'$ 插到中间时，新的差分数组的变化如下：
1. 第1项 $d_1$ 删去（因为 $a_1$ 变成了 $a_1'$ 后移）
2. 原本的 $d_j$ 被替换为 $d_j'=d_1-\sum\limits_{k=2}^{j-1}d_k$ ，$d'_{j+1}=d_j-d_j'$ 。

第2点变化可能比较难理解，给出如下例子：
- $a = [1,6,8,10,13,15]$ （原序列排序去重）
- $d = [5,2,2,3,2]$ （差分数组）
- $a_1=1,a_2=6,d_1=5$ （选定次小元素做操作1）
- $a_1'=a_2+d_1=11$
- $a' = [6,8,10,11,13,15]$ （新序列排序去重，$a_1'$ 位于第4位）
- $d' = [2,2,1,2,2]$ （新差分数组）

差分数组的变化：
1. 第1项 $d_1=5$ 删去
2. 第4项 $3$ 变为：$d_1-d_2-d_3=1$ 和 $3-1=2$

这个变化的意义就在于，它证明了任意次操作后的差分数组中的元素，是原差分数组中元素的线性组合，且随着操作次数增加，$\sum d_i$ 逐渐减小，直到 $a$ 仅剩2个元素。
线性组合能达到的最小值为 $\gcd\limits_{i=1}^{n-1}d_i$ 。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    create_vec(v,n);
    SORT(v);
    ll ans = 0;
    FORLL(i,1,n-1)
        ans = __gcd(ans,v[i]-v[i-1]);
    cout << ans << endl;
}
```

# G.Horse Drink Water
## 题意
将军饮马问题，将军在第一象限的整点 $(x_0,y_0)$ ，河流由 $x$ 正半轴和 $y$ 正半轴组成。
问将军碰到河流再前往 $(x_1,y_1)$ 的最短路径长度。

## 解题思路
将起点以 $x$ 轴、 $y$ 轴为对称轴，分别对称到第四、二象限，比较这两个点和终点的距离，取最小值。

## 参考程序
```cpp
void solve()
{
    ld xx1,xx2,yy1,yy2;
    cin >> xx1 >> yy1 >> xx2 >> yy2;
    ld ans1,ans2;
    ans1 = sqrt((xx1-xx2)*(xx1-xx2)+(yy1+yy2)*(yy1+yy2));
    ans2 = sqrt((xx1+xx2)*(xx1+xx2)+(yy1-yy2)*(yy1-yy2));
    print_float(min(ans1,ans2),10);
    cout << endl;
}
```

# I
// TODO