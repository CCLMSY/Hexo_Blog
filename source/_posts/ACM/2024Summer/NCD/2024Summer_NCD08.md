---
title: 题解|2024暑期牛客多校08
date: 2024/8/8
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD08
description: 题解|2024暑期牛客多校08
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

比赛链接：[2024牛客暑期多校训练营8](https://ac.nowcoder.com/acm/contest/81603)

# A.Haitang and Game
## 题意
给定一个包含 $n$ 个数的正整数集合 $S$ ，每次可以从中选择两个数 $x,y$ 满足 $x,y\in S,\gcd(x,y)\notin S$，将 $\gcd(x,y)$ 加入 $S$ 。
最多加入数的个数是奇数输出`dXqwq`，否则输出`Haitang`。

## 解题思路
考虑最终集合中的数 $d$ ，一定满足原集合中，它的所有倍数的 $gcd$ 等于它。
从小到大枚举 $d$ ，如果 $d$ 满足上述条件且不在原集合中，计数。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    unordered_set<ll> s;
    ll mx=-1,t;
    FORLL(i,1,n){
        cin >> t;
        s.insert(t);
        chmax(mx,t);
    }
    ll cnt=0;
    FORLL(i,1,mx){
        ll fl=0;
        for(ll j=i;j<=mx;j+=i)
            if(s.count(j)) fl=__gcd(fl,j);
        if(fl==i&&!s.count(i)) cnt++;
    }
    if(cnt%2) cout << "dXqwq\n";
    else cout << "Haitang\n";
}
```

# J.Haitang and Triangle
## 题意
构造一个长度为 $n$ 的排列，它恰有 $m$ 个长度为 $3$ 的子区间满足子区间的三个数能构成非退化三角形。

## 解题思路
构造一个长度为 $n-m$ 的排列，它不含能够构成非退化三角形的长度为 $3$ 的子区间。
我使用的方法是把 $1\sim n-m$ 从大到小均分成三组，第1组降序，其他组升序，按321的顺序放置。
假设 $n-m=7$ ，分成 $[2,1],[3,4],[5,6,7]$ ，排列为 $5,3,2,\ 6,4,1,\ 7$ 。

剩下 $m$ 个数降序放在排列的前面，和上面的排列合并形成 $m$ 个合法区间。

## 参考代码
```cpp
void solve()
{
    ll n,m;cin >> n >> m;
    ll t = n-m;
    if(t<3){
        cout << "-1" << endl;
        return;
    }
    deque<ll> v[3];
    ll t1=t/3,t2=t%3;
    FORLL(i,1,t1) v[0].emplace_back(i);
    FORLL(i,t1+1,t1*2+(t2>1)) v[1].emplace_front(i);
    FORLL(i,t1*2+(t2>1)+1,t) v[2].emplace_front(i);
    // FORLL(i,0,2) print_vec(v[i]);
    vector<ll> ans;
    FORLL(i,0,t1){
        if(i<v[2].size()) ans.emplace_back(v[2][i]);
        if(i<v[1].size()) ans.emplace_back(v[1][i]);
        if(i<v[0].size()) ans.emplace_back(v[0][i]);
    }
    FORLL_rev(i,n,t+1) cout << i << Presentation(i,1);
    FORLL(i,0,t-1) cout << ans[i] << Presentation(i,t-1);
}
```

# K.Haitang and Ava
## 题意
判断给定字符串是否仅由若干个 `ava` 和 `avava` 组成。

## 解题思路
从 $i=1$ 开始，对于当前位置开头，先检查长度为 $5$ 的子串是否是 `avava` ，再检查长度为 $3$ 的子串是否是 `ava` ，如果是，就跳过这个子串；否则输出`NO`。

## 参考代码
```python
pat1 = 'ava'
pat2 = 'avava'

T = int(input())
for _ in range(T):
    s = input()
    i = 0
    while i < len(s):
        if i+5<=len(s) and s[i:i+5] == pat2:
            i += 5
        elif i+3<=len(s) and s[i:i+3] == pat1:
            i += 3
        else:
            print('No')
            break
    else:
        print('Yes')
```