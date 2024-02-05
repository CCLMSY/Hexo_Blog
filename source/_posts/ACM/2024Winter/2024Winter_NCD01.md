---
title: 题解|2024牛客寒假01
date: 2024/2/2
tags:
  - 2024牛客寒假
categories: 算法竞赛
top_img: false
comments: true
toc: true
abbrlink: e5822278
description: 题解|2024牛客寒假01
updated:
keywords:
cover:
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

# A.DFS搜索
**1签**
## 题目大意
题目字面意思，问能不能在长度为 $n$ 的字符串 $s$ 中找到子序列"DFS"和"dfs"；
## 解题思路
直接暴力搜索
时间复杂度 $O(n)$
## 参考程序
```cpp
void solve()
{
    string s,s1,s2;
    ll n;cin >> n;
    cin >> s;
    s1="DFS";s2="dfs";
    int cnt1,cnt2;cnt1=cnt2=0;
    for(auto c:s){
        if(cnt1<3&&c==s1[cnt1]) cnt1++;
        if(cnt2<3&&c==s2[cnt2]) cnt2++;
    }
    if(cnt1>=3) cout << 1; else cout << 0; cout << ' ';
    if(cnt2>=3) cout << 1; else cout << 0; cout << endl;
}
```

# B.关鸡
**模拟**
## 题目大意
有一条宽为$2$，长为$2*10^9+1$的管道，每个格子的坐标标记为$(x,y)$，$x\in[-10^9,10^9],y\in [1,2]$
管道内有$n$个障碍，给定障碍的坐标
鸡哥在$(0,1)$处，可以上下左右移动，但不能穿过障碍
求最少还需多少个障碍物才能使鸡哥被困在管道内无法到达管道两端
## 解题思路
将管道两排看作两侧，如果某处有障碍（下图红），只要它的另一侧相邻的位置中有一个有障碍（下图黄）即可堵住管道：
![alt text](/images/ACM/2024Winter_NCD01_B.png)
换个角度，从管道左端到右端，对于每个障碍，check它对面且与它的$x$距离不大于$1$的位置是否有障碍，即可确定是否堵住管道。

要把鸡哥困在管道内，需要把鸡哥两侧管道全都堵住。

对于鸡哥的一侧管道，如果：
  没有堵住，没有障碍，则需放置$2$个障碍；
  没有堵住，至少有$1$个障碍，则需放置$1$个障碍；
  已经堵住，则不需要放置障碍。

特别的，和鸡哥直接相邻的3个位置如果都有障碍，鸡哥就直接被困住了。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    vector<pll> v(n);
    int f,fl,fr; f=fl=fr=0;
    for(auto &x:v){
        cin >> x.second >> x.first; // 为了方便排序，将x轴坐标放在前面
        if(x.first==-1&&x.second==1) f++;
        if(x.first==0&&x.second==2) f++;
        if(x.first==1&&x.second==1) f++;//和鸡哥直接相邻的障碍数
        if(x.first<=0) fl=1;//鸡哥左侧有障碍
        if(x.first>=0) fr=1;//鸡哥右侧有障碍
    }
    SORT(v);
    int ffl,ffr;ffl=ffr=0;//记录左右两侧是否被堵住
    deque<pll> dq;
    FORLL(i,0,n-1){
        while(dq.size()&&v[i].first-dq.front().first>1) dq.pop_front();//dq只保存[x-1,x]内的障碍
        for(auto x:dq){
            if(x.second!=v[i].second){//如果有不同侧的障碍即堵住管道
                if(v[i].first<=0) ffl=1;
                else ffr=1;
            }
        }dq.push_back(v[i]);
    }
    ll ans=3-f;//直接围堵鸡哥
    if(ffl&&ffr) ans=min(ans,0ll);//左右两侧都被堵住，无需添加
    else if(ffl&&fr||fl&ffr) ans=min(ans,1ll);//一侧被堵住，另一侧有障碍，需要添加1个
    else if(ffl||ffr) ans=min(ans,2ll);//一侧被堵住，另一侧无障碍，需要添加2个
    else if(fl&&fr) ans=min(ans,2ll);//两侧没被堵住，但有障碍，需要添加2个
    //其余情况不如直接围堵鸡哥
    cout << ans << endl;
}
```

# C.按闹分配
**贪心**
## 题目大意
$n$个人在$1$个窗口前排队办事，第$i$个人办事需要时间$t_i$
开始为$0$时刻，每个人的不满意度$D_i$为其办完事的时刻$D_i$
工作人员安排队伍顺序使得所有人的不满意度之和 $S=\sum D_i$ 最小

你也来办事，需要时间$t_c$。你可以插队，但是因此增加的不满意度之和不能超过$M$
问你最早什么时候能办完事

## 解题思路
根据贪心的思想，初始使得不满意度之和最小的排序是按照$t_i$从小到大排序
你插队导致的不满意度之和的增量为：你插队的位置之后的人数*你的办事时间
因此你后面的人数不得超过 $M/t_c$ 个
在这个约束下计算前缀和即可

## 参考程序
```cpp
void solve()
{
    ll n,Q,tc,M;
    cin >> n >> Q >> tc;
    create_vec(t,n);
    SORT(t);
    vector<ll> St;
    St.pb(0);
    FORLL(i,0,n-1) St.pb(St.back()+t[i]);
    while(Q--){
        cin >> M;
        ll x=M/tc;
        ll pl=max(0ll,n-x);
        cout << St[pl]+tc << endl;
    }
}
```

# E.本题又主要考察了贪心
**DFS、诈骗**
## 题目大意
$n$个人参加比赛，当前第$i$个人已经得到了$a_i$分，接下来还有$m$轮比赛
每轮两个人PK,赢的人$+3$分；平局则每人$+1$分
给定$m$轮比赛的名单，问$1$号选手能取得的最高名次

## 解题思路
直接贪心很难贪（反正我是贪不出来）
由于人数和局数很少，直接DFS到每种结局，找到最优解即可
时间复杂度：$O(3^m)$

## 参考程序
```cpp
vector<pll> q; //记录每轮比赛的参赛选手
ll n,m,ans;
void DFS(vector<ll> a,ll i){
    if(i==m){
        ll cnt=0;
        for(auto x:a) if(x>a[0]) cnt++;
        ans=min(ans,cnt);
        return ;
    }
    vector<ll> aa=a,ab=a,ac=a;
    aa[q[i].first-1]+=3;//a赢
    DFS(aa,i+1);
    ab[q[i].second-1]+=3;//b赢
    DFS(ab,i+1);
    ac[q[i].first-1]+=1;//平局
    ac[q[i].second-1]+=1;
    DFS(ac,i+1);
}
void solve()
{
    cin >> n >> m;
    ans=INF;
    vector<ll> a(n);
    for(auto &x:a) cin >> x;
    q.clear();
    ll u,v;
    FORLL(i,1,m){
        cin >> u >> v;
        if(u>v) swap(u,v);
        q.pb({u,v});
    }
    DFS(a,0);
    cout << ans+1 << endl;
}
```

# F.鸡数题！
**概率论-排列组合**
## 题目大意
将$n$位的最大二进制数$111\cdots 1$（n个1）的每一位$1$分配给$m$个数，且每个数都不为$0$，问有多少种分配方案
## 解题思路
$n$个不同的球放入$m$个不同的盒子，每个盒子至少一个球，有多少种放法
答案为第二类斯特林数$S(n,m)$
通项公式：$S(n,m)=\sum\limits_{i=0}^m\dfrac{(-1)^{m-i}\times i^n}{i!\times (m-i)!}$
## 参考程序
```cpp
ll pown[N]={0};
void solve()
{
    ll m,n;cin >> n >> m;
    if(n<m) {cout << 0 << endl;return;}
    FORLL(i,0,m){
        pown[i]=qcpow(i,n);
    }
    Prepare_Factorium(m);
    ll ans=0;
    FORLL(i,0,m){
        ll t=mul(pown[i],mul(Fac_inv[i],Fac_inv[m-i]));
        if((m-i)&1) t=MOD-t;
        addto(ans,t);
    }cout << ans << endl;
}
```

# G.why买外卖
**贪心**
## 题目大意
有$n$张外卖券，第$i$张满$a_i$减$b_i$，$a_i$为餐品原价。
所有券可以叠加使用，你手上有$m$元，问你最多可以购买到原价为多少的餐品
## 解题思路
假设餐品原价为$x$，则所有$a_i<x$的券都可以使用。
根据使用门槛$a_i$对券排序，对减免部分做前缀和。
在每个门槛处计算：该门槛需支付的价格$x$（或溢出的优惠）=当前门槛-减免金额
如果$x$小于$m$，则更新答案，当前门槛下最高餐品价格为$m+减免金额$
## 参考程序
```cpp
void solve()
{
    ll n,m;cin >> n >> m;
    vector<pll> vp(n);
    for(auto &x:vp) cin >> x.first >> x.second;
    SORT(vp);
    vector<ll> S(n+1);S[0]=0;
    FORLL(i,1,n) S[i]=S[i-1]+vp[i-1].second;
    ll ans=m;
    FORLL(i,1,n){
        if(vp[i-1].first-S[i]<=m){
            ans=max(ans,S[i]+m);
        }
    }cout << ans << endl;
}
```

# I.It's bertrand paradox. Again!
**概率论**
## 题目大意
现在有两种在$x\in [-100,100],y\in [-100,100]$平面范围内生成（圆心在整点上且半径为整数的）圆形的算法（仅第3步不同）：

1. 均匀随机生成一个 $(-100,100)$ 内的整点 $(x,y)$
2. 均匀随机生成一个 $[1,100]$ 内的整数半径 $r$

bit的检验：不满足在范围内，返回第2步，即仅重新生成$r$
buaa的检验：不满足在范围内，返回第1步，即重新生成一个圆

现在给出其中某个算法的生成 $10^5$ 个圆的结果，问是由哪个算法生成的

## 解题思路
两种算法下，生成结果的圆心和半径的分布是不同的。
明显bit的方法圆心是均匀分布在平面内的，而buaa的方法不是。

根据抽样分布原理，大量独立同分布随机变量和的极限分布是正态分布。因此对圆心