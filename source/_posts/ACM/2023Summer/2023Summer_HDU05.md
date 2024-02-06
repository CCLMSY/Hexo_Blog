---
title: 题解|2023暑期杭电多校05
date: 2023/8/1
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: f4d5a717
description: 题解|2023暑期杭电多校05
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

# 1001.Typhoon
**计算几何**
## 题目大意
依次给定 $n$ 个坐标 $P$ ，预测的台风路线为按顺序两两连接给定坐标所得的折线
现在有 $m$ 个庇护所的坐标 $S$ ，求每个庇护所到台风路线的最短距离
![Img](/images/ACM/2023Summer_HDU05_1001.jpg)
## 解题思路
对于每个庇护所坐标，求它到每个路线线段的距离，再取最短即可
常见的错误为计算了点到线段所在直线的距离，而非到线段的距离（~~别问我怎么知道的~~）

问题转移到如何计算点到线段的距离
可以考虑将线段表示为向量，记为 $vec$ ;点到线段两端的向量记为 $vec1,vec2$ 
如果 $vec1,vec2$ 与 $vec$ 的点积同时大于 $0$ 或同时小于 $0$ ，则距离在端点取到，否则距离为点到直线的距离//
具体说明可以参考代码注释

### 时间复杂度
 $O(nm)$

## 参考代码
```cpp
vector<pll> typh,shel;
vector<double> frac,dist;
double cal_dist(pll s,pll t1,pll t2){//
    ll x,y,x1,y1,x2,y2,p1,p2;
    pll vec,vec1,vec2;double re;
    x=s.first;y=s.second;//庇护所坐标
    x1=t1.first;y1=t1.second;//端点1
    x2=t2.first;y2=t2.second;//端点2
    vec={x2-x1,y2-y1};vec1={x-x1,y-y1};vec2={x-x2,y-y2};
    //线段向量、点到两个端点的向量
    p1=vec.first*vec1.first+vec.second*vec1.second;//点积1
    p2=vec.first*vec2.first+vec.second*vec2.second;//点积2
    if(p1<=0&&p2>0||p1>0&&p2<=0){//点到直线距离
        double frac=(double)sqrt((y2-y1)*(y2-y1)+(x1-x2)*(x1-x2));
        re=(double)fabs((y2-y1)*x+(x1-x2)*y-y2*x1+x2*y1)/frac;
    }else{//点到端点距离
        double dist1=(double)sqrt(vec1.first*vec1.first+vec1.second*vec1.second);
        double dist2=(double)sqrt(vec2.first*vec2.first+vec2.second*vec2.second);
        re=min(dist1,dist2);
    }
    return re;
}
void solve()
{
    ll n,m;
    cin >> n >> m;
    typh.resize(n);shel.resize(m);dist.resize(m);
    for(auto &[x,y]:typh) cin >> x >> y;
    for(auto &[x,y]:shel) cin >> x >> y;
    FORLL(i,0,m-1){dist[i]=INF;
        FORLL(j,0,n-2)
            dist[i]=min(dist[i],cal_dist(shel[i],typh[j],typh[j+1]));}
    for(double i:dist) {print_float(i,4);cout << endl;}
}
```

***

# 1006.Touhou Red Red Blue
**贪心/DP**
## 题目大意
你将依次收到 $n$ 个物品，他们具有颜色红、绿或蓝，记为 $R,G,B$

你有一个大小为 $2$ 的物品栏。每当你收到一个物品，你可以考虑将其放入物品栏**或直接丢弃**
物品栏是一个栈，这意味着如果你决定放入物品但物品栏是满的，你将丢弃更早放入的那个物品，并将手上的物品放入

下面是物品的得分和消除规则：
1. 如果手上和物品栏中共 $3$ 个物品颜色相同，则消除这 $3$ 个物品，得到 $1$ 分（这是唯一的得分方式），并在物品栏得到 $1$ 个颜色由你决定的新物品
2. 如果手上和物品栏中共 $3$ 个物品颜色各不相同，则消除这 $3$ 个物品，并在物品栏得到 $2$ 个颜色由你决定的新物品
   
求对于给定的物品序列，可以得到的最高分是多少

## 解题思路
官方给出的做法是DP，但是考虑所有的状态转移略显繁琐，难以不重复、不遗漏
相比起来我还是更喜欢直接贪心的方法//

决定一种游戏策略前，先掌握游戏核心机制
首先，在贪心的思想下，物品栏的栈特性可以不考虑//因为如果需要被迫丢弃物品，大可以在拿到这个物品时就直接丢弃//
其次，注意到颜色任选的物品可以不立刻考虑，作为`任意物品`即可
考虑消除过程中的特点：
1. 规则 $2$ 消除后得到 $2$ 个`任意物品`，可以配合规则 $1$ ，直接和下一个物品（若有）合成得分
2. 规则 $1$ 消除后留下 $1$ 个`任意物品`。这意味着如果有得分，最后一定会留下 $1$ 个`任意物品`

借助上面两个特点，单独考虑第一次消除，后续消除借助前面得到的`任意物品`，具体可以参考代码注释
计数直到满足条件消除，即视为取需要的物品，并直接丢弃无帮助的物品，重新计数即可

### 时间复杂度
 $O(n)$

## 参考代码
```cpp
int RGB(char c){
    switch (c)
    {
        case 'R':return 1;
        case 'G':return 2;
        default:return 3;
    }return 0;
}
void solve()
{
    string s;cin >> s;
    ll n=s.length(),res=0;
    int cur=0,cnt[4]={0};
    FORLL(i,0,n-1){
        cur=RGB(s[i]);cnt[cur]++;
        if(res){//消除过，前面必定有一个自选
            if(cnt[1]&&cnt[2]||cnt[1]&&cnt[3]||cnt[2]&&cnt[3]){
                //已有两种不同颜色+自选：选不同颜色，消3得2
                if(i+1==n) break;//没有下一个就结束
                i++;res++;//再用2个自选和下一个出现的颜色消除
                cnt[1]=cnt[2]=cnt[3]=0;
            }else if(cnt[1]==2||cnt[2]==2||cnt[3]==2){
                //已有两个相同颜色+自选：选相同颜色，消3得1
                res++;cnt[1]=cnt[2]=cnt[3]=0;
            }
        }else{//还没有消除过
            if(cnt[1]&&cnt[2]&&cnt[3]){//兼有三色：消3得2
                if(i+1==n) break;
                i++;res++;//再用2个自选和下一个出现的颜色消除
                cnt[1]=cnt[2]=cnt[3]=0;
            }else if(cnt[1]==3||cnt[2]==3||cnt[3]==3){//有三个同色：消3得1
                res++;cnt[1]=cnt[2]=cnt[3]=0;
            }
        }
    }cout << res << endl;
}
```

***

# 1007.Expectation (Easy Version)
**数学期望**
## 题目大意
玩 $n$ 次游戏，每次游戏胜利的概率为 $\dfrac{a}{b}$ 
对于给定的 $m$ ，每次游戏胜利将获得 $k^m$ 分，其中 $k$ 是当前总胜利局数，失败得 $0$ 分
最终得分为 $n$ 次游戏的得分之和，求最终得分的期望

## 解题思路
从全局角度考虑得分情况，游戏输赢概率满足二项分布，记获胜概率为 $p=\dfrac{a}{b}$
在 $n$ 局游戏中赢 $k$ 局的概率为： $C_n^k\cdot p^k(1-p)^{n-k}$
赢 $k$ 局游戏可以获得的最终得分记为 $score(k)$ ： $score(k)=\sum\limits_{i=1}^k i^m$
故最终得分期望为： $E=\sum\limits_{i=1}^n C_n^k\cdot p^k(1-p)^{n-k}\cdot score(k)$

分别预处理阶乘、乘方、得分数组，求和取模即可

### 时间复杂度
乘方、阶乘、求和取模： $O(n)$
得分（快速幂+前缀和）： $O(n\log m)$
总时间复杂度： $O(n\log m)$

## 参考代码
> 参考代码为已AC代码主干，其中功能请读者自行实现

```cpp
vector<ll> powp,pownp,score;//p的n次、q的n次、赢n局的得分
void solve()
{
    ll n,m,a,b,re=0;
    cin >> n >> m >> a >> b;
    ll p=mul(a,inv(b));//获胜的概率
    ll np=Get_Mod(1-p);//失败的概率
    powp.clear();pownp.clear();score.clear();
    powp.resize(n+1);pownp.resize(n+1);score.resize(n+1);
    powp[0]=pownp[0]=1;score[0]=0;
    FORLL(i,1,n){
        powp[i]=mul(powp[i-1],p);
        pownp[i]=mul(pownp[i-1],np);
        score[i]=add(score[i-1],qcpow(i,m));}
    FORLL(i,1,n)
        re=add(re,mul(score[i],mul(Get_Combination(n,i),mul(powp[i],pownp[n-i]))));
        //Ex(i)=C(n,i)*p^i*(1-p)^(n-i)*score(i)
    cout << re << endl;
}
```

***

# 1012.Counting Stars
**计数**
## 题目大意
定义 $k$ 星状图：一个 $k+1$ 个节点， $k$ 条边的无向图，中心节点与其他 $k$ 个节点之间各有一条边，其他节点之间没有边 
给定一个无向图 $G=(V,E),|V|=n,|E|=m$ ，记其中不同的 $k$ 星状图的数量为 $cnt_k$
求对于 $2\le k\le n-1$ ，所有 $cnt_k$ 的异或和： $cnt_2\oplus cnt_3\oplus \cdots \oplus cnt_{n-1}$

## 解题思路
对于某个节点 $i$ ，记它的度为 $deg_i$ ，考虑以它作为中心节点的情况
简单的组合问题，以 $i$ 为中心节点的 $k$ 星图数量即 $C_n^k$
遍历 $n$ 个节点，在每个节点处对 $2\le k\le deg_i$ 的 $k$ 星图计数取模
最终把所有结果做异或和即可

### 时间复杂度
预处理阶乘（计算组合数）： $O(n)$
遍历节点：$O(n+m)$
异或和：$O(n)$
总时间复杂度：$O(n+m)$


## 参考代码
```cpp
vector<ll> deg,cnt;
void solve()
{
    ll n,m;
    cin >> n >> m;
    deg.clear();deg.resize(n+1);cnt.clear();cnt.resize(n+1);
    ll u,v;
    FORLL(i,1,m){
        cin >> u >> v;
        deg[u]++;deg[v]++;
    }
    FORLL(i,1,n)
        FORLL(j,2,deg[i])
            cnt[j]=Get_Mod(cnt[j]+Get_Combination(deg[i],j));
    ll re=0;
    FORLL(i,2,n-1) re^=cnt[i];
    cout << re << endl;
}
```