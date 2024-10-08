---
title: 题解|2023暑期杭电多校01
date: 2023/7/18
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_HDU01
description: 题解|2023暑期杭电多校01
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

# 1002.City Upgrading

**树形DP、支配集**

## 题意
crazyzhk居住在一个树形城市。有一天，这个城市的网络需要升级。为了实现这一目标，需要部署路由器。
每个路由器可以覆盖其所在的节点及其相邻节点，在节点 $i$ 上放置一个路由器的成本是 $a_i$ 
确保每个节点都得到覆盖的情况下，成本最低是多少

### 前置知识点
- [树形DP](https://oi-wiki.org/dp/tree/)
- [支配集](https://baike.baidu.com/item/%E6%94%AF%E9%85%8D%E9%9B%86/3187787)：给定无向图 $G=(V,E)$ ，称 $V$ 的一个子集 $S$ 为支配集，当且仅当对于 $V-S$ 中任何一个点 $v$ ，都有 $S$ 中的某个点 $u$ ，使得 $(u,v) \in E$ 

## 解题思路
要实现信号全覆盖，所选取的点集必须是一个**支配集**
题目求的是所有支配集中最小的cost，考虑**树形DP**

构造DP数组```dp[N][3]```:
1. ```dp[i][0]```表示点 $i$ 属于支配集合，并且以点 $i$ 为根的子树都被覆盖了的情况下，支配集中所包含最少cost
2. ```dp[i][1]```表示点 $i$ 不属于支配集合，且以 $i$ 为根的子树都被覆盖，且 $i$ 被其不少于一个子节点覆盖的情况下，支配集所包含最少cost
3. ```dp[i][2]```表示点 $i$ 不属于支配集合，且以 $i$ 为根的子树都被覆盖，且 $i$ 没被子节点覆盖（将被父节点覆盖）的情况下，支配集中所包含最少cost

结合最小支配集的贪心思想，按**DFS序的逆序**进行DP，节点 $i$ 的子节点记为 $u$

对于第一种状态，```dp[i][0]```含义为点 $i$ 属于支配集合，那么依次取每个儿子节点三种状态中的最小值，再把取得的最小值全部加起来再加1，就是dp```[i][0]```的值了。即只要每个以 $i$ 的儿子为根的子树都被覆盖，再加上当前点 $i$ ，所需要的最少cost，DP转移方程如下：
$$
dp[i][0] = 1 + ∑min(dp[u][0], dp[u][1], dp[u][2])
$$
对于第三种状态，```dp[i][2]```含义为点 $i$ 不属于支配集合，且 $i$ 被其父节点覆盖。那么说明点 $i$ 和点 $i$ 的儿子节点都不属于支配集合，所以点 $i$ 的第三种状态之和其儿子节点的第二种状态有关，方程为：
$$
dp[i][2] =∑dp[u][1]
$$
对于第二种状态，略有些复杂。
首先如果点 $i$ 没有子节点那么```dp[i][1]```应该初始化为INF；否则为了保证它的每个以 $i$ 的儿子为根的子树被覆盖，那么要取每个儿子节点的前两种状态的最小值之和，因为此时点 $i$ 不属于支配集，不能支配其子节点，所以子节点必须已经被支配，与子节点的第三种状态无关。
如果当前所选状态中每个儿子都没被选择进入支配集，即在每个儿子的前两种状态中，第一种状态都不是所需点最小的，那么为了满足第二种状态的定义(因为点 $i$ 的第二种状态必须被其子节点覆盖，即其子节点必须有一个属于支配集，如果此时没有，就必须强制使一个子节点的状态为状态一)，需要重新选择点 $i$ 的一个儿子节点为第一种状态，这时取花费最少的一个点，即取```min(dp[u][0] - dp[u][1])```的儿子节点 $u$ ，强制取其第一种状态，其他的儿子节点取第二种状态，DP转移方程为:
$$dp[i][1] = \begin{cases}
    INF ，i没有子节点 \newline
    ∑min(dp[u][0], dp[u][1]) + inc ，Otherwise \newline
\end{cases} \newline
$$
$$
其中，inc = \begin{cases}
    0 ，上式的Otherwise中至少选择了一次状态一的子节点 \newline
    min(dp[u][0] - dp[u][1]) ，Otherwise
\end{cases} \newline
$$

结果为根节点三个状态中的最小值

### 时间复杂度
$O(nlogn)$

## 参考程序
```cpp
#define N 2005
vector<vector<ll>> T;
vector<ll> cost;
vector<ll> DFS_vec;
void DFS(ll node){
    DFS_vec.emplace_back(node);
    for(auto i:T[node]) DFS(i);
}//获取DFS遍历序列
int solve()
{
    ll n;
    cin >> n;
    T.clear();T.resize(n+1);
    cost.clear();cost.resize(n+1);

    FORLL(i,1,n) cin >> cost[i];
    ll u,v;
    FORLL(i,1,n-1){
        cin >> u >> v;
        T[u].emplace_back(v);
    }
    
    DFS_vec.clear();
    DFS(1);
    reverse(ALL(DFS_vec));//逆序
    //print_vec(DFS_vec);
    
    ll dp[N][3]={0};
    for(auto i:DFS_vec)
    {
        dp[i][0]=cost[i];
        dp[i][1]=0;
        dp[i][2]=0;
        for(auto j:T[i])
        {
            dp[i][0]+=min({dp[j][0],dp[j][1],dp[j][2]});
            dp[i][2]+=dp[j][1];
        }

        if(T[i].empty()) dp[i][1]=INF;
        else{
            int flag=1;ll inc=INF;
            for(auto j:T[i])
            {
                if(dp[j][0]<=dp[j][1]) {dp[i][1]+=dp[j][0];flag=0;}
                else dp[i][1]+=dp[j][1];
            }
            if(flag){
                for(auto j:T[i]) inc=min({inc,dp[u][0]-dp[u][1]});
                dp[i][1]+=inc;
            }
        }
    }

    ll re=min({dp[1][0],dp[1][1]});
    cout << re << endl;

    return 0;
}
```

***

# 1005.Cyclically Isomorphic

**字符串匹配**

## 题意
对于两个字符串 $s_1$ , $s_2$ ，如果存在一个整数 $k$ 使得 $s_1$ 循环右移k位与 $s_2$ 相同，则称他们是"cyclically right-shifted"。
每组数据给出 $n$ 个长度为 $m$ 的字符串， $Q$ 次询问两个字符串是否"cyclically right-shifted"。

## 解题思路
对于每一字符串，找到其字典序最小的状态储存，每次询问时直接比较即可。
主要是时间复杂度难以证明

### 时间复杂度
找最小态储存 $n \times m^2$ ，比较 $Q \times m$
 $Q \le \dfrac{1}{2} n(n-1) \Rightarrow$ 比较 $m \times n^2$  
总复杂度： $O(mn \times (m+n))$ ， 
 $mn \le 10^5$ 下~~数据不变态的话~~就能卡过去

## 参考程序
```cpp
int solve()
{
    string s[N],s1,s2;
    char t;
    ll n,m;
    cin >> n >> m;
    FORLL(i,1,n)
    {
        cin >> s1;
        s2=s1;
        FORLL(j,1,m)
        {
            t=s2.front();
            s2.erase(s2.begin());
            s2.push_back(t);
            if(s1.compare(s2)>0) s1=s2;
        }
        s[i]=s1;
    }
    ll Q;cin >> Q;
    while(Q--)
    {
        cin >> n >> m;
        if(s[n].compare(s[m])==0) cout << YES;
        else cout << NO;
    }
    return 0;
}
```

***

# 1009.Assertion

**签到题**

## 题意
多组数据
给定m件物品，分成n组
问：是否无论怎么分都至少有一组个数超过d

## 解题思路
平均分组，找最大那个组的个数和d比较

## 参考程序
```cpp
int solve()
{
    ll m,n,d;
    cin >> n >> m >> d;
    if((m-1)/n+1>=d) cout << YES;
    else cout << NO;
    return 0;
}
```
