---
title: 题解|2024暑期杭电多校06
date: 2024/8/5
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU06
description: 题解|2024暑期杭电多校06
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（6）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A86%A3%A9&source=1&searchmode=source)

# (1001)HDU7494 造花（简单版）
## 题意
给定一棵 $n$ 个节点的树，问能否通过删去一个节点，使得剩下的节点组成2个连通块，且每个连通块都是一个菊花图。

菊花图是一棵树，且存在唯一中心点与其他所有节点之间有一条边。特殊的，只有一个节点的树也是菊花图。

## 解题思路
一棵树删去一个点得到两个连通块，那么删去的点的度必定为2。

这个删去的点 $u$ 和得到的两个菊花图的中心点 $v_1,v_2$ 有三种情况：
1. $u$ 和 $v_1,v_2$ 直接相连
2. $u$ 和 $v_1$ 直接相连，和 $v_2$ 的距离为2
3. $u$ 和 $v_1,v_2$ 的距离都为2

记录度大于1的节点，这些点是关键节点，可能是中心点，也可能是删去的点。

度大于1的节点数最多的情况是第三种，最多有5个度大于1的节点，形如：
```
  8       9
  |       |
1-2-3-4-5-6-7
  |       |
  10      11
```

删去点 $u$ 后，剩余度大于1的节点必定是中心点，个数不大于 $2-t$ 即满足。 
其中， $t$ 是删点后仅由不多于$2$个点组成的连通块的个数。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    vector<vector<ll>> G(n+1);
    ll u,v;
    FORLL(i,1,n-1){
        cin >> u >> v;
        G[u].emplace_back(v);
        G[v].emplace_back(u);
    }
    vector<ll> crit; //关键点
    FORLL(i,1,n) if(G[i].size()>1) crit.emplace_back(i);
    if(crit.size()>5) {cout << "No\n";return;} 
    for(auto u:crit) if(G[u].size()==2){ //枚举删除的点
        ll cnt2=0,cnt1=0;
        for(auto v:G[u]){ //相邻的2个点
            if(G[v].size()==2){ 
                for(auto w:G[v])  //w==u:这一端的连通块只有2个节点
                if(w!=u) if(G[w].size()>1) cnt2++;
            }
            if(G[v].size()==1) cnt1++; //这一端的连通块只有1个节点
        }
        ll t=crit.size()-1-cnt2;
        if(t<=2-cnt1) { cout << "Yes\n"; return;}
    }
    cout << "No\n";
}
```

# (1003)HDU7496 飞车狂飙
## 题意
给定一个长度为 $n$ 的字符串，包含：
1. `L` ：表示左转的方块
2. `R` ：表示右转的方块
3. `S` ：表示直行的方块

从原点开始，按照字符串给定的顺序，先放置，再按照给定的方向移动。

合法的字符串满足：
1. 不在同一位置放置两个方块
2. 所有方块形成一个环

问给定的字符串是否合法。

## 解题思路
模拟。
每走一步更新沿途放置了方块的坐标和当前方向。
中途检查是否有重复放置的方块，最后检查是否回到原点、方向是否回正。

## 参考代码
```cpp
//        0 1 2 3
int dx[]={0,-1,0,1};
int dy[]={1,0,-1,0};
void solve()
{
    pll cur={0,0};
    int dir=0; //0:up 1:left 2:down 3:right
    ll n;cin >> n;
    string s;cin >> s;
    map<pll,int> mp;
    for(auto c:s){
        if(mp.count(cur)){  cout << "-1\n"; return; }
        mp[cur]=1;
        if(c=='L') dir=(dir+1)%4;
        else if(c=='R') dir=(dir+3)%4;
        cur.first+=dx[dir];
        cur.second+=dy[dir];
    }
    if(cur.first==0&&cur.second==0&&dir==0) cout << "1\n";
    else cout << "0\n";
}
```

# (1004)HDU7497 不醒人室
## 题意
给定 $n$ 个上课时间段， $m$ 个睡觉时间段。

初始状态是不清醒。
每个睡觉时间段 $[s,t]$ 能使接下来 $2(t-s)$ 的时间，也就是 $[t,t+2(t-s)]$ 时间段内保持清醒。
睡觉提供的清醒时间段不会叠加，以此前最后一个睡觉时间段为准。

问给定的时间段能否满足以下条件：
1. 上课时间保持清醒
2. 上课时间不能睡觉

## 解题思路
将所有时间段排序，从左往右遍历：
1. 当前是上课时间段，检查：
   1. 上课开始时间是否晚于最后一个睡觉时间段的结束时间
   2. 上课结束时间是否早于最后一次睡觉提供的清醒时间段的结束时间
2. 当前是睡觉时间段，检查睡觉时间是否晚于上一节课的结束时间

## 参考代码
```cpp
void solve()
{
    ll n,m;cin >> n >> m;
    vector<pair<pll,int>> v(m+n);
    FORLL(i,0,n-1){
        cin >> v[i].first.first >> v[i].first.second;
        v[i].second = 0; //上课
    }
    FORLL(i,n,n+m-1){
        cin >> v[i].first.first >> v[i].first.second;
        v[i].second = 1; //睡觉
    }
    SORT(v);
    ll b,e,s,t; //上次上课时间[b,e]，上次睡觉时间[s,t]
    b=e=s=t=0;
    for(auto [tp,op]:v){
        auto [x,y] = tp;
        if(op==0){ //上课
            if(x<t){ cout << "No\n";return;} //上课开始还在睡觉
            if(t+2*(t-s)<y) {cout << "No\n";return;} //上次没睡够支撑不到下课
            b=x;e=y;
        }else{ 
            if(x<e){ cout << "No\n";return;} //还没下课就睡觉
            s=x;t=y;
        }
    }
    cout << "Yes\n";
}
```

# (1005)HDU7498 交通管控
## 题意
有 $k$ 盏红绿灯，每盏灯有三种状态：绿色`A`、黄色`B`、红色`C`。

一个操作用一个长度为 $k$ 的字符串表示，一个字符对应一盏灯。
字符串中包含：
1. `+`：红变绿、绿变黄、黄变红
2. `-`：绿变红、黄变绿、红变黄
3. `0`：不变

交警有一个长度为 $n$ 的操作序列，他可以按顺序对每个操作选择执行或不执行。

问最后能达到哪些状态，以及每种状态对应的 操作序列种数。

## 解题思路
状态压缩DP。

用一个 $k$ 位3进制数表示 $k$ 盏灯的状态，每一位表示一盏灯的状态：0绿、1黄、2红。
记：
- $dp[i][x]$ 表示前 $i$ 个操作，状态为 $x$ 的方案数。
- $op(x,i)$ 表示状态 $x$ 经过第 $i$ 个操作后的状态。

状态转移方程：$dp[i][x]=dp[i-1][x]+\sum\limits_{j}^{st\in stat[i-1],op(st,i)=j} dp[i-1][st]$
其中，$stat[i-1]$ 表示第 $i-1$ 个操作后的所有状态。

利用滚动数组可以实现空间优化。

## 参考代码
```cpp
vector<ll> pow3;
void prepare(){
    pow3.emplace_back(1);
    FORLL(i,1,20) pow3.emplace_back(pow3.back()*3);
}
ll n,k;
//在x的3进制，第idx位，+d再对3取模
ll opadd(ll x,ll idx,ll d){
    if(d==0) return x;
    ll t = x/pow3[idx]%3;
    t = (t+d+3)%3-t;
    return x + t*pow3[idx];
}
//0A1B2C
string trans(ll x){
    string s;
    FORLL(i,0,k-1){
        s += char('A'+x%3);
        x /= 3;
    } return s;
}
void solve()
{
    cin >> n >> k;
    cin >> MODULE::MOD;
    vector<string> v(n);
    for(auto& s:v) cin >> s;
    map<ll,ll> dp[2];
    int fl=0;
    dp[fl].insert({0,1}); fl^=1;
    for(auto& s:v){
        dp[fl]=dp[fl^1];
        for(auto [v,cnt]:dp[fl^1]){
            ll t=v;
            FORLL(i,0,k-1){
                if(s[i]=='+') t=opadd(t,i,1);
                if(s[i]=='-') t=opadd(t,i,-1);
            }
            addto(dp[fl][t],cnt);
        }
        fl^=1;
    }fl^=1;
    map<string,ll> ans;
    for(auto [v,cnt]:dp[fl]) ans.insert(make_pair(trans(v),(ll)cnt));
    for(auto [s,cnt]:ans) cout << s << ' ' << cnt << endl;
}
```