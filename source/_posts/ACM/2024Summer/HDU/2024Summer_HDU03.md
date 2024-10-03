---
title: 题解|2024暑期杭电多校03
date: 2024/7/26
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU03
description: 题解|2024暑期杭电多校03
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（3）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A83%A3%A9&source=1&searchmode=source)

# (1001)HDU7457.深度自同构
## 题意
求由 $n$ 个节点构成，且满足“深度相同的节点，度相同”的森林的方案数。

## 解题思路
先考虑 $i$ 个节点的合法的树，这棵树每个节点的子树形态相同。
去掉根节点，每个部分的子树形态相同，那么每个子树的节点数是 $i-1$ 的约数。
设 $f(i)$ 表示 $i$ 个节点满足上述条件的树的方案数，那么有：$f(i)=\sum_{d|(i-1)} f(d)$

再考虑 $n$ 个节点的合法的森林，每个树的节点数是 $n$ 的约数。
设 $g(n)$ 表示 $n$ 个节点满足上述条件的森林的方案数，那么有：$g(n)=\sum_{d|n} f(d)$

## 参考程序
```cpp
const ll N = 1e6+5;
ll f[N],g[N];
void prepare(){
    f[1]=1;
    ll n=1e6;
    FORLL(i,1,n)
        for(ll j=i;j<=n;j+=i)
            addto(f[j+1],f[i]);
    FORLL(i,1,n)
        for(ll j=i;j<=n;j+=i)
            addto(g[j],f[i]);
    //寄巧：对于从约数i到倍数j的转移，可以枚举倍数k，以调和级数复杂度求
}
void solve()
{
    ll n;cin >> n;
    FORLL(i,1,n) cout << g[i] << Presentation(i,n);
}
```

# (1007)HDU7463.单峰数列
## 题意
给定一个长度为 $n$ 的数列 $a$ ，$q$ 次操作：
1. `1 l r x`：$a[l,r]$ 每个数加 $x$
2. `2 l r`：判断 $a[l,r]$ 是否相同
3. `3 l r`：判断 $a[l,r]$ 是否严格单调递增，$l=r$ 时也算
4. `4 l r`：判断 $a[l,r]$ 是否严格单调递减，$l=r$ 时也算
5. `5 l r`：判断 $a[l,r]$ 是否单峰数列

长度为 $m$ 的数列 $b$ 被称为单峰数列，当且仅当存在唯一位置 $1\lt i\lt m$ ，使得 $b[1,i]$ 严格单调递增，$b[i,m]$ 严格单调递减。

## 解题思路
暴力判断
维护差分序列更直观

## 参考程序
```cpp
vector<ll> v;
ll n;
void range_add(ll l,ll r,ll val){
    FORLL(i,l-1,r-1) v[i]+=val;
}
int check_same(ll l,ll r){
    FORLL(i,l,r-1) if(v[i]!=v[l-1]) return 0;
    return 1;
}
int check_up(ll l,ll r){
    FORLL(i,l,r-1) if(v[i-1]>=v[i]) return 0;
    return 1;
}
int check_down(ll l,ll r){
    FORLL(i,l,r-1) if(v[i-1]<=v[i]) return 0;
    return 1;
}
int check_mountain(ll l,ll r){
    int fl=0; //up
    if(v[l-1]>v[l]) return 0;
    FORLL(i,l,r-1){
        if(v[i-1]==v[i]) return 0;
        if(fl==0){
            if(v[i-1]>v[i]) fl=1;
        }else if(fl==1){
            if(v[i-1]<v[i]) return 0;
        }
    }
    if(fl==0) return 0;
    return 1;
}
void solve()
{
    cin >> n;
    v.resize(n);
    for(auto &x:v) cin >> x;
    ll q;cin >> q;
    ll op,l,r,val;
    while(q--){
        cin >> op;
        if(op==1){
            cin >> l >> r >> val;
            range_add(l,r,val);
        }else if(op==2){
            cin >> l >> r;
            cout << check_same(l,r) << endl;
        }else if(op==3){
            cin >> l >> r;
            cout << check_up(l,r) << endl;
        }else if(op==4){
            cin >> l >> r;
            cout << check_down(l,r) << endl;
        }else if(op==5){
            cin >> l >> r;
            cout << check_mountain(l,r) << endl;
        }
    }
}
```

# (1008)HDU7464.比特跳跃
## 题意
给定一个 $n$ 个点 $m$ 条边（不保证连通，可能有重边）的无向图。
每条边 $E(u,v)$ 有一个权值 $w$ ，表示从 $u$ 到 $v$ 的代价。
此外，对于任意两点 $u,v$ ，可以付出 $k\times(u|v)$ （按位或）的代价从 $u$ 跳到 $v$ 。
分别求从节点 $1$ 到所有其他点 $2$ ~ $n$ 的最小代价。

## 解题思路
最暴力的想法肯定是把直接跳跃也看作边加入图中，然后跑单源最短路。
但是这样边的数量是 $n^2$ ，显然不可取。
因此考虑哪些边是有必要加的。

$1$ 直接跳到其他节点的方案是很优的：
若节点 $i$ 不在节点 $1$ 所在的连通块内，那么 $1$ 直接跳到 $i$ 的代价只可能是 $i$ （$i$ 为奇数）或 $i+1$ （$i$ 为偶数）。

但如果存在 $E(1,2,0)$ ，对于 $x=2,6,10...$ 这一类满足 $x\%4=2$ 的节点， $1$ 直接跳到 $x$ 的代价为 $x+1$ ，但先走到 $2$ 再跳到 $x$ 的代价仅为 $x$ 。

因此，对于 $x\%4=2$ 的节点，需要加入 $E(2,x,k(x|2))$ 。

同理，对于 $x\%2^{i+1}=2^i$ 的节点，需要加入 $E(2,x,k(x|2^i))$ 。

最后，跑一遍单调队列优化的单源最短路，求出答案。

## 参考程序
```cpp
vector<ll> pow2(1,1);
void prepare(){
    for(ll i=1;i<=60;i++) pow2.emplace_back(pow2.back()*2);
}
void solve()
{
    ll n,m,k;cin >> n >> m >> k;
    Dijkstra dij(n);
    FORLL(i,1,m){
        ll u,v,w;
        cin >> u >> v >> w;
        dij.addedge(u,v,w);
        dij.addedge(v,u,w);
    }
    FORLL(i,2,n) dij.addedge(1,i,k*(1|i));
    FORLL(i,1,60){
        if(pow2[i]>n) break;
        for(ll j=pow2[i]*2;j<=n;j+=pow2[i]) if(pow2[i]&j)
            dij.addedge(pow2[i],j,k*(pow2[i]|j));
    }
    dij.solve(1);
    FORLL(i,2,n) cout << dij[i] << Presentation(i,n);
}
```

# (1011)HDU7467.抓拍
## 题意
$n$ 个人，第 $i$ 个人初始在 $(x_i,y_i)$ 。
每个人有一个行进方向：东（E,+x方向）、西（W,-x方向）、南（S,-y方向）、北（N,+y方向），每秒移动一个距离。
散步无限长时间、不改变方向、忽略碰撞。

求一个周长最小的矩形满足：
1. 上下边平行于 $x$ 轴，左右边平行于 $y$ 轴
2. 存在一个时刻，所有人都在矩形内部

## 解题思路
每个时刻的最小周长矩形只由 最上面、最下面、最左边、最右边 的四个人决定，那么就考虑可能成为这四个人的人。

左右移动的人占据的y区间，上下移动的人占据的x区间，不会改变。
记录：左右走的人里最靠上、最靠下的人，上下走的人里最靠左、最靠右的人。

因为时间无限长，最后在最上面的人，一定是向上走的人中初始点最靠上的，其他方向同理。
记录：向上走的人里最靠上的、向下走的人里最靠下的，向左走的人里最靠左的、向右走的人里最靠右的。

开始最靠下的人如果是向左/右/下走的，已经记录了，只要在向上走的人中找到最靠下的，其他方向同理。
记录：向上走的人里最靠下的、向下走的人里最靠上的、向左走的人里最靠右的、向右走的人里最靠左的。

有了这12个值，就可以计算出任何一个时刻的 $x_{min},x_{max},y_{min},y_{max}$ ，从而计算出最小周长。

模拟可知，周长对时间的函数，要么是单调增的，要么是一个开口向上的二次函数（因为最后人一定越走越散）。因此可以三分找到极小值点，即为答案。

## 参考程序
```cpp
//           E W S N
//           R L D U
int dx[4] = {1,-1,0,0};
int dy[4] = {0,0,-1,1};
bool cmp(pll a,pll b){
    if(a.second==b.second) return a.first<b.first;
    return a.second<b.second;
}
ll yLRmax,yLRmin,xUDmax,xUDmin;
ll xLmin,xLmax,xRmin,xRmax,yUmin,yUmax,yDmin,yDmax;
ll getC(ll t){
    ll xmin = min({xLmin-t,xRmin+t,xUDmin});
    ll xmax = max({xLmax-t,xRmax+t,xUDmax});
    ll ymin = min({yDmin-t,yUmin+t,yLRmin});
    ll ymax = max({yDmax-t,yUmax+t,yLRmax});
    return 2*((xmax-xmin)+(ymax-ymin));
}
void solve()
{
    ll n;cin >> n;
    char c;
    vector<pll> L,R,U,D;
    ll x,y;
    FORLL(i,1,n){
        cin >> x >> y >> c;
        if(c=='E') R.emplace_back(x,y);
        if(c=='W') L.emplace_back(x,y);
        if(c=='N') U.emplace_back(x,y);
        if(c=='S') D.emplace_back(x,y);
    }
    //左右移动的最上和最下两个人，上下移动的最左和最右两个人
    sort(ALL(L),cmp); sort(ALL(R),cmp); sort(ALL(U)); sort(ALL(D));
    yLRmax = xUDmax = -INF;
    yLRmin = xUDmin = INF;
    if(L.size()) {chmax(yLRmax,L.back().second);chmin(yLRmin,L.front().second);}
    if(R.size()) {chmax(yLRmax,R.back().second);chmin(yLRmin,R.front().second);}
    if(U.size()) {chmax(xUDmax,U.back().first);chmin(xUDmin,U.front().first);}
    if(D.size()) {chmax(xUDmax,D.back().first);chmin(xUDmin,D.front().first);}
    //初始状态，向左的最左最右，向右的最左最右，向上的最上最下，向下的最上最下
    sort(ALL(L)); sort(ALL(R)); sort(ALL(U),cmp); sort(ALL(D),cmp);
    xLmin = xRmin = yUmin = yDmin = INF;
    xLmax = xRmax = yUmax = yDmax = -INF;
    if(L.size()) {chmin(xLmin,L.front().first);chmax(xLmax,L.back().first);}
    if(R.size()) {chmin(xRmin,R.front().first);chmax(xRmax,R.back().first);}
    if(U.size()) {chmin(yUmin,U.front().second);chmax(yUmax,U.back().second);}
    if(D.size()) {chmin(yDmin,D.front().second);chmax(yDmax,D.back().second);}
    ll l=0,r=2e9;
    //三分找到getC最小的t
    while(r-l>10){
        ll m1 = l+(r-l)/3;
        ll m2 = r-(r-l)/3;
        if(getC(m1)<getC(m2)) r=m2;
        else l=m1;
    }
    ll mnans=INF;
    FORLL(i,l,r) chmin(mnans,getC(i));
    cout << mnans << endl;
}
```

# (1012)HDU7468.死亡之组
## 题意
给定一个长度为 $n$ 的数组 $a$ （$n$ 是4的倍数），$a_i$ 表示第 $i$ 个队伍的实力。
每4个队伍一组，“死亡之组”至少符合以下条件之一：
1. 至少有2个队伍的实力 $> L$
2. 最大实力和最小实力之差 $\le D$

问是否存在一种分组方案使得第 $1$ 组所在的小组不是“死亡之组”。

## 解题思路
为了不满足条件1，存在至少3个队伍的实力 $\le L$ 即可。
然后把这3个队伍 $t_1,t_2,t_3$ 拿出来。
为了不满足条件2，所有队伍的实力中，最大值和最小值之差 $> D$ 即可。
然后把实力最大的队伍 $t_4$ 拿出来。

对队伍1（$a_1$）进行讨论：
- 如果 $a_1>L$，替换 $t4$ ，重新判定
- 如果 $a_1\le L$，替换 $t3$

## 参考程序
```cpp
void solve()
{
    ll n,l,d;cin >> n >> l >> d;
    create_vec(v,n);
    ll t1 = v[0];
    SORT(v);
    ll cntll=0;
    for(auto x:v) if(x<l) cntll++;
    if(cntll<3){
        cout << "No" << endl;
        return ;
    }
    if(t1>=l){
        if(t1-v.front()<=d) cout << "No" << endl;
        else cout << "Yes" << endl;
        return ;
    }else{
        if(v.back()-v.front()<=d) cout << "No" << endl;
        else cout << "Yes" << endl;
        return ;
    }
}
```
