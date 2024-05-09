---
title: 题解|2024牛客寒假01
tags:
  - 2024牛客寒假
categories: 算法竞赛
mathjax: true
abbrlink: e5822278
date: 2024/2/2
description: 题解|2024牛客寒假01
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

# D.数组成鸡
**思维**
## 题目大意
给定一个长度为$n$的整数数组，每次操作可以使所有元素都$+1$或都$-1$。
$Q$次询问，问任意次操作后能否使数组所有元素的乘积等于给定的数$M$（$-1e9\le M \le 1e9$）。
## 解题思路
询问的$M$范围不大，所以数组稍微长一点儿，就很可能溢出$1e9$的范围。

由于元素都是整数，若数组中绝对值大于$1$的元素的个数超过$20$个，那么乘积的绝对值最小为$2^20$，超过$1e9$。

枚举出现过的数字$x_i$，再向$x_i$两边枚举它附近的数字$t$，使全数组$-t$，然后直接计算数组的乘积进行$check$。
当乘积的绝对值已经超过$1e9$，即出现最多$20$个绝对值大于$1$的元素时，直接判不合法，$check$枚举的元素个数是比较少的。
若最终乘积的绝对值不大于$1e9$，则加入答案集合。

询问时在答案中二分查找即可。

## 参考程序
```cpp
void solve()
{
    ll n,Q,t;cin >> n >> Q;
    vector<ll> a(n);
    set<ll> ans; ans.insert(0);
    set<ll> exi;
    for(auto &x:a){
        cin >> x;
        exi.insert(x);
    }
    auto check = [&](ll x) -> bool{
        ll tt=1;
        for(auto &y:a){
            tt*=y+x;
            if(tt==0) return true;
            if(abs(tt)>1e9) return false;
        }ans.insert(tt);
        return true;
    };
    for(auto &x:exi){
        t=-x-1; while(check(t)) t--;
        t=-x+1; while(check(t)) t++;
    }
    while(Q--){
        cin >> t;
        if(ans.count(t)) cout << "Yes\n";
        else cout << "No\n";
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

# H.01背包，但是bit
**位运算**
## 题目大意
有$n$个物品，第$i$个物品的价值为$v_i$，重量为$w_i$，每个物品只有一个。
选定物品的重量之和定义为这些物品各自重量的“或”和，即$w=w_1|w_2|w_3|\cdots|w_c$；价值之和定义为这些物品各自重量的初等代数和：$v=\sum v_i$
问选定物品的重量之和不超过$m$的情况下，价值之和的最大值
## 解题思路
或运算的性质：有$1$出$1$，全$0$出$0$。

把所有“重量”看作二进制。

假设选定物品后结果为$x$，那么$x$的每个$1bit$的位置，一定有某个被选中的物品的重量在该位上是$1bit$。

枚举所有允许出现$1$的位置的所有情况。
将$m$的某一$1bit$置$0$，则该位的低位可以任意取值，高位不变。
如$m=1010000B$，将第二个$1bit$置$0$，则$1bit$的合法位置变为$1001111$。
确定了$1bit$的合法位置，那每个物品只能是选或不选。
遍历$m$的所有$1bit$，计算将其置$0$的情况：遍历所有物品，应选尽选。
取所有情况的最大值。

## 参考程序
```cpp
vector<ll> v,w;
ll n,m,ans;
void Update(ll x){
    ll t=0;
    FORLL(i,0,n-1)
        if((w[i]&x)==w[i])
            t+=v[i];
    ans=max(ans,t);
}
void solve()
{
    cin >> n >> m; ans=0;
    v.resize(n); w.resize(n);
    FORLL(i,0,n-1) cin >> v[i] >> w[i];
    FORLL(i,0,29) if(m&(1<<i)){
        Update((m^(1<<i))|((1<<i)-1));
        //m^(1<<i)：m的第i位置0
        //(1<<i)-1：低i位全1
    }Update(m);
    cout << ans << endl;
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

根据抽样分布原理，大量独立同分布随机变量和的极限分布是正态分布。
因此对样本建立统计量，使得两种算法下，该统计量有显著不同即可。
此处建立的统计量$U$为：圆心到原点的距离的均值$A(\sqrt{x^2+y^2})$。
用两种算法生成样本，暴算得到$U_{buaa}\approx56，U_{bit}\approx 75$。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    double sum=0;
    ll x,y;
    FORLL(i,1,n)
    {
        cin >> x >> y;
        sum+=sqrt(x*x+y*y);
        cin >> x;
    }sum/=n;
    if(sum<65) cout << "buaa-noob" << endl;
    else cout << "bit-noob" << endl;
}
```

# J.又鸟之亦心
**思维**
## 题目大意
两个人分别在数轴的$x,y$位置。
接下来$n$天，第$i$天必须从两人中选择一人到$a_i$的位置。
记这$n$天内两人最远的距离为$D$，决策使得$D$最小，求这个最小的$D$
## 解题思路
二分答案$D$，判断是否存在一种决策使得两人最远距离不超过$D$。

忽略两人的身份，第$i$天一定有一个人的位置在$a_i$，记录另一个人可能的位置集合$s$。
对于二分点，遍历每个$a_i$，更新$s$，使得$s$中的点与$a_i$的距离不超过$D$。
如果$s$为空，说明不存在一种决策使得两人最远距离不超过$D$。

## 参考程序
```cpp
const ll N = 200005;
void solve()
{
    ll n,x,y;cin >> n >> x >> y;
    create_vec(a,n);
    auto check = [&](ll d) -> bool{
        set<ll> s;
        ll lst=y;
        if(abs(x-y)>d) return false;
        s.insert(x);
        for(auto t:a){
            if(s.empty()) return false;
            if(abs(t-lst)<=d) s.insert(lst);
            while(!s.empty()&&*s.begin()<t-d) s.erase(s.begin());
            while(!s.empty()&&*s.rbegin()>t+d) s.erase(*s.rbegin());
            lst=t;
        }return !s.empty();
    };
    ll l=0,r=1e9;
    while(l<r){
        ll mid=(l+r)/2;
        if(check(mid)) r=mid;
        else l=mid+1;
    }cout << l << endl;
}
```

# K.牛镇公务员考试
**图论**
## 题目大意
有$n$道题，每道题的题目为：“第$a_i$题的答案是（）？”
每题有`A B C D E`五个选项，选项的内容是字符串`ABCDE`的一个排列，记为字符串$s_i$
例如第$i$题的题目为：

第$a_i$题的答案是（）？
A. $s_i[0]$ B. $s_i[1]$ C. $s_i[2]$ D. $s_i[3]$ E. $s_i[4]$

求答对所有题目的方案数

## 解题思路
将题目看作节点。
第$i$道题决定了第$a_i$道题的答案，视为一条从$i$到$a_i$的有向边。
同时，每个节点的出度为$1$，这意味着每个连通分量都是一个内向基环树。
（内向基环树：每个节点的出度为$1$的弱连通图）

反过来看，一旦确定了第$a_i$道题的答案，第$i$道题的答案也就确定了。
因此对于一条链，只要确定了一个节点的答案，整条链的答案也就唯一确定了。
显然在内向基环树中，链一定是挂在某个环上，链的答案随环的选择而唯一确定，因此可以忽略链。

对于环，先确定一个节点的答案（5种），然后递推确定整个环的答案，判断是否合法。
每个环的方案数在$0-4$之间

最终答案为每个连通分量方案数的乘积

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n ;
    vector<ll> a(n);
    vector<string> s(n);
    FORLL(i,0,n-1){
        cin >> a[i] >> s[i];
        a[i]--;
    }
    ll ans=1;
    vector<int> vis(n,-1);
    FORLL(i,0,n-1){
        ll j=i;
        while(vis[j]==-1){
            vis[j]=i;
            j=a[j];
        }
        if(vis[j]!=i) continue;
        vector<ll> cycle;//环
        ll k=j;
        do{
            cycle.push_back(k);
            k=a[k];
        }while(k!=j);
        ll res=0,t;
        FORLL(i,0,4){
            t=i;
            for(auto x:cycle) t=s[x][t]-'A';
            if(t==i) res++;
        }
        multo(ans,res);
    }cout << ans << endl;
}
```

# L.要有光
**诈骗题**
## 题目大意
给了一些几何形状的方程，求在**地面**（$z=0$）上的最大阴影面积

## 解题思路
题目拐弯抹角吓唬人///
保证符合题意的状态是光源处于$xOy$平面上($z=0$)。
目的是求一个$xOy$平面上的梯形阴影的面积。
上底长$2w$，下底长$4w$，高为$c$，面积为$3wc$。

## 参考程序
```cpp
void solve()
{
    ll c,d,h,w;
    cin >> c >> d >> h >> w;
    cout << w*3*c << endl;
}
```

# M.牛客老粉才知道的秘密
**2签**
## 题目大意
某场比赛一共有$n$道题目，但每面只能显示$6$道。
点击`下一页`时，如果后面的题目足够$6$题，则显示下面的$6$题，否则显示最后6题。
点击`上一页`时，如果前面的题目足够$6$题，则显示上面的$6$题，否则显示最前6题。
求比赛中可能出现在第一题位置的题目数量。

## 解题思路
如果题目数量是$6$的倍数，那页面可能的题目排布为$n/6$。
如果题目数量不是$6$的倍数，
从第一页到最后一页，可能的题目排布为$n/6+1$；
从最后一页到第一页，可能的题目排布为$n/6+1$。
去除首页尾页，可能的题目排布为$(n/6)*2$。

## 参考程序
```cpp
#define N 200005
void solve()
{
    ll n;cin >> n;
    if(n%6==0) cout << n/6 << endl;
    else cout << n/6*2 << endl;
}
```