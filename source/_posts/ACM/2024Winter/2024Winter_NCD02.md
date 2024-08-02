---
title: 题解|2024牛客寒假02
tags:
  - 2024牛客寒假
categories: 算法竞赛
mathjax: true
abbrlink: ACM_2024Winter_NCD02
date: 2024/2/5
description: 题解|2024牛客寒假02
---

# A.Tokitsukaze and Bracelet
**1签**
## 题意
装备有$a,b,c$三个值，每个值分别对应一个强化等级，求装备的总强化等级。

## 解题思路
根据每项数值判断强化等级，然后求和即可。

## 参考程序
```cpp
void solve()
{
    ll a,b,c;
    cin >> a >> b >> c;
    ll ans=0;
    if(a==150) ans+=1;
    else if(a==200) ans+=2;
    if(b>32&&b<41) ans+=1;
    else if(b>42) ans+=2; 
    if(c>32&&c<41) ans+=1;
    else if(c>42) ans+=2;
    cout << ans << endl;
}
```

# B.Tokitsukaze and Cats
**模拟**
## 题意
上场关鸡，这场关猫//
有$n$只猫，在$(n,m)$大小的矩阵中，每只猫占据一个格子，坐标给定。
每只猫的四面需要围上围栏，求需要多少根围栏。

## 解题思路
模拟，一只一只放入，根据已经放入的猫的位置，判断需要增加多少根围栏。

## 参考程序
```cpp
void solve()
{
    ll m,n,k;
    cin >> n >> m >> k;
    int v[305][305]={0};
    ll x,y,cnt;
    ll ans=0;
    FORLL(i,1,k){
        cin >> x >> y;
        v[x-1][y-1]=1;
        cnt=0;
        if(x>1&&v[x-2][y-1]) cnt++;
        if(x<n&&v[x][y-1]) cnt++;
        if(y>1&&v[x-1][y-2]) cnt++;
        if(y<m&&v[x-1][y]) cnt++;
        switch(cnt)
        {
            case 0: ans+=4;break;
            case 1: ans+=3;break;
            case 2: ans+=2;break;
            case 3: ans+=1;break;
            default: break;
        }
    }cout << ans << endl;
}
```

# D.Tokitsukaze and Slash Draw
**图论**
## 题意
有一堆$n$张卡牌的牌堆。
有$m$种操作，第$i$种操作可以将牌堆顶的前$a_i$张卡牌按原本的顺序放置在牌堆底，代价为$b_i$.
求将牌堆**从下往上数**第$k$张卡牌放到牌堆顶的最小代价。

## 解题思路
将牌堆看作一个有向图，$n$个位置代表$n$个节点。
第$i$种操作可以看作从每个顶点引出一条指向向后$a_i$个节点的边，边权为$b_i$。
求从第$n-k$个节点（从上往下数）到第$0$个节点的最短路即可。
由于总边数来到了$mn=5\times10^6$，最短路算法可以用$O(m\log n)$的Dijkstra。

## 参考程序
```cpp
struct edge {
  ll v, w;
};
struct node {
  ll dis, u;
  bool operator>(const node& a) const { return dis > a.dis; }
};
vector<edge> e[N];
ll dis[N], vis[N];
priority_queue<node, vector<node>, greater<node>> q;
void dijkstra(ll n, ll s) {
    FORLL(i,0,n){
        dis[i] = INF;
        vis[i] = 0;
    }
    dis[s] = 0;
    q.push({0, s});
    while (!q.empty()) {
        ll u = q.top().u;
        q.pop();
        if (vis[u]) continue;
        vis[u] = 1;
        for (auto ed : e[u]) {
            ll v = ed.v, w = ed.w;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                q.push({dis[v], v});
            }
        }
    }
}
void solve()
{
    ll n,m,k;cin >> n >> m >> k;
    ll a,b,t;
    FORLL(i,0,n) e[i].clear();
    FORLL(i,1,m)
    {
        cin >> a >> b;
        FORLL(i,0,n-1) e[i].push_back((edge){(i+a)%n,b});
    }
    dijkstra(n,0);
    if(dis[n-k]==INF) cout << -1 << endl;
    else cout << dis[n-k] << endl;
}
```

# E.F.Tokitsukaze and Eliminate(easy+hard)
**贪心**
## 题意
$n$个宝石排成一排，第$i$个宝石的颜色为$col_i$
每次操作可以任选一种颜色，消除这种颜色的最右端的宝石和它右边的所有宝石。
问最少需要多少次操作才能消除所有宝石。

## 解题思路
贪心，从右往左扫描，直到从当前位置到最右端。
这一区间内的宝石，包含了当前剩余的所有颜色，操作消除。
当前剩余颜色可在读入时处理前缀和

## 参考程序
```cpp
void solve()
{
    ll n,t;cin >> n;
    vector<ll> v,sc;//sc[i]记录位置i之前出现过的颜色数量
    v.pb(0);sc.pb(0);
    map<ll,int> mp;
    FORLL(i,1,n){
        cin >> t;
        v.pb(t);
        if(mp[t]==0) {sc.pb(sc[i-1]+1);mp[t]++;}
        else sc.pb(sc[i-1]);
    }
    mp.clear();
    t=0;//t记录当前的颜色数量
    ll r=n,ans=0;//r记录当前剩余宝石数量
    FORLL_rev(i,n,1){
        if(mp[v[i]]==0){
            t++;
            mp[v[i]]=1;      
        }
        if(t==sc[r]){
            ans++;
            t=0;mp.clear();
            r=i-1;
        }
    }cout << ans << endl;
}
```

# G.Tokitsukaze and Power Battle (esay)
**数据结构**
## 题意 (esay version)
有一个长度为$n$的**非负**整数数组，有$q$次操作：
1. `1 i x`：将第$i$个数改为$x$
2. `2 l r`：查询区间$[l,r]$内，每个长度不小于$2$的**子区间**，任意分成连续两段后，`左段之和减去右段之和`的最大值。

## 解题思路
实现单点修改、区间查询的数据结构，可以选用线段树或树状数组（本人采用树状数组）。
根据贪心的思想，由于数组非负，要最大化`左段之和减去右段之和`，子区间的左端点选定查询区间的左端点。
从右往左枚举右端点，并在分段时，右段只需留下一个数即可。

## 参考程序
```cpp
#define lowbit(x) ((x)&(-(x)))//取最后一个1所在位置的权值
struct BITree{//树状数组，下标i从1开始
    vector<ll> Data;
    explicit BITree(ll n):Data(n*2+5,0) {}
    void update(ll i,ll dif)
    {//给予i增量dif,维护树状数组，O(logn)
        while(i<Data.size()){
            Data[i]+=dif;
            i+=lowbit(i);
        }
    }
    ll presum(ll i)
    {//查询前缀和sum[i]，O(logn)
        ll sum=0;
        while(i){
            sum+=Data[i];
            i-=lowbit(i);
        }
        return sum;
    }
    ll query(ll l,ll r){//查询区间和
        return presum(r)-presum(l-1);
    }
};
void solve(){
    ll n,Q,t;
    cin >> n >> Q;
    BITree bt(n);
    for(ll i=1;i<=n;i++){
        cin >> t;
        bt.update(i,t);//维护原数组，实现单点修改，区间查询
    }//建树
    ll ans;
    while(Q--){
        int op;
        ll l,r;
        cin >> op >> l >> r;
        if(op==1){
            r=r-bt.query(l,l);
            bt.update(l,r);
            t=1;
        }else{
            ans=-INF;
            FORLL_rev(i,r,l+1){//枚举右端点
                if(ans>=bt.query(l,i)) break;
                //优化：如果当前答案已经大于接下来的区间前缀和，则不必再继续枚举
                ans=max(ans,bt.query(l,i-1)-bt.query(i,i));
            }
            cout << ans << endl;
            t=1;
        }
    }
}
```

# I.Tokitsukaze and Short Path (plus)
**思维**
## 题意
给定一个有$n$个点的完全图（每两点之间有一条边），每个点$i$具有点权$a_i$。
每两个点之间的边权为这两点点权**较大**的值的2倍。
求每两个点之间的最短路之和：$\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}dist(i,j)$
## 解题思路
$n\le 2e5$ 的数据范围，首先排除建图暴算

对于任意两点$u,v$：
假设$u>v$，那么$e(u,v)$（$u,v$之间的直接边）的权值为$2a_u$。
除$u$之外的任意点$w$，$e(u,w)$的权值为$2max\{a_u,a_w\}\ge a_u$，因此经过任意点$w$再到$u$的路径都不会比$e(u,v)$更短。
因此，对于任意两点$u,v$，$e(u,v)$是$u$到$v$的最短路。

对所有点权升序排序，点权的位次也是这个点权对答案贡献的次数。
时间复杂度：$O(n)$

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    create_vec(v,n);
    SORT(v);
    ll ans=0;
    FORLL(i,0,n-1) ans+=v[i]*i;
    cout << ans*4 << endl;
    //*4的含义：边权有系数2；每两个点互相到达要计算2次
}
```

# J.Tokitsukaze and Short Path (minus)
**思维**
## 题意
给定一个有$n$个点的完全图（每两点之间有一条边），每个点$i$具有点权$a_i$。
每两个点之间的边权为这两点点权**较小**的值的2倍。
求每两个点之间的最短路之和：$\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}

## 解题思路
对于任意两点$u,v$：
假设$u<v$，那么$e(u,v)$（$u,v$之间的直接边）的权值为$2a_v$。
又假设$u,v$都不是点权最小的点，对于图中点权最小的点$w$，$u\rightarrow v\rightarrow w$的路径长度为$4a_w$（两条边）。
那么$dist(u,v)=min\{4a_w,2a_v\}$
因此，每个点为$min\{2a_w,a_v\}$在对答案做贡献

对所有点权降序排序，点权的位次也是这个点（$min\{2a_w,a_v\}$）对答案贡献的次数

## 参考程序
```cpp
void solve()
{
    ll n,t;cin >> n;
    vector<ll> v;
    ll mn=INF;
    FORLL(i,1,n)
    {
        cin >> t;
        mn=min(mn,t);
        v.pb(t);
    }
    SORT(v);
    ll ans=0;
    FORLL(i,0,n-1) ans+=min(mn*2,v[i])*(n-i-1);
    cout << ans*4 << endl;
    //*4的含义：边权有系数2；每两个点互相到达要计算2次
}
```

# K.Tokitsukaze and Password (easy)
**暴力**
## 题意
给定一个长度为$n$的字符串$x$，其中包含数字、小写字母和下划线‘_’，用来表示一个纯数字密码。
密码满足以下条件：
1. 密码没有前导$0$
2. 密码可以被$8$整除
3. 给定另一个长度为$n$的数字$y$，保证$x\lt y$
4. 每种字母表示$[0,9]$中的一个，且不同字母表示的数字必定不同
5. 每一位下划线‘_’都可以表示$[0,9]$中的任意一个数字，下划线上的数字不必相同

求可能成为密码的数量

## 解题思路
由于easy version的数据范围极小，暴力枚举判断是否满足条件即可
(srd有点考验耐心细心qwq)

## 参考程序
```cpp
ll n,y;
string sx;
vector<int> ub;//记录待定字符的取值上界
int pending(vector<int>& vk){
    ll x=0;
    if(n>1)
        if(sx[0]=='0') return 0;
        else if(isalpha(sx[0])&&vk[sx[0]-'a']==0) return 0;
        else if(sx[0]=='_'&&vk[4]==0) return 0;//不含前导0
    FORLL(i,0,3) if(ub[i])
        FORLL(j,i+1,3) if(ub[j]&&vk[i]==vk[j]) return 0;//不同字母取值不同
    for(auto c:sx){
        x*=10;
        if(isdigit(c)) x+=c-'0';
        else if(isalpha(c)) x+=vk[c-'a'];
        else x+=vk[4];
    }
    if(x%8) return 0;//不是8的倍数
    if(x>y) return 0;//大于y
    return 1;
}
void solve(){
    string sy;
    cin >> n >> sx >> sy;
    y=stoll(sy);
    ub.clear();
    ub.resize(10,0);
    for(auto c:sx) switch(c){
        case 'a':ub[0]=9;break;
        case 'b':ub[1]=9;break;
        case 'c':ub[2]=9;break;
        case 'd':ub[3]=9;break;
        case '_':ub[4]=9;break;
    }
    vector<int> vk;
    ll ans=0;
    FORLL(a,0,ub[0]) FORLL(b,0,ub[1]) FORLL(c,0,ub[2]) FORLL(d,0,ub[3]) FORLL(e,0,ub[4]){
        vk.clear();
        vk.pb(a);vk.pb(b);vk.pb(c);vk.pb(d);vk.pb(e);
        ans+=pending(vk);
    }cout << ans << endl;
}
```