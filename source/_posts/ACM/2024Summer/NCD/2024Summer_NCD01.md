---
title: 题解|2024暑期牛客多校01
date: 2024/7/16
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD01
description: 题解|2024暑期牛客多校01
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

# A.A Bit Common

**组合数学**

## 题目大意
**题目概括：**
给定两个整数 $n$ 和 $m$，我们需要计算所有包含 $n$ 个非负整数且每个整数都小于 $2^m$ 的序列中，满足**存在**一个非空子序列使得这些整数的按位与（bitwise AND）为 1 的序列的数量。

注意：一个序列的非空子序列是指从序列中删除零个或多个元素后得到的子序列，并保持剩余元素的原始顺序。

由于结果可能非常大，请输出结果对一个正整数 $q$ 取模后的值。

## 解题思路
对于序列 $A$ 中的每个整数，我们可以将其看作一个 $m$ 位的二进制数。

符合条件的序列中，所有末位为 1 的元素的&和一定为 1 ，因为其他元素的末位为 0 ，再做&运算会消灭末位 1 。

枚举序列包含的末位为 1 的元素个数 $i$ 。

对于这 $i$ 个元素，要满足除末位外每一个二进制位不全为 1 。这 $i$ 个数每个二进制位上的方案数为 $2^{i}-1$ ，除末位外有 $m-1$ 个二进制位，（有序）方案数为 $a=(2^{i}-1)^{m-1}$ 。 

对于剩余的 $n-i$ 个元素，只需要保证末位为 0 。可能的数字有 $2^{m-1}$ 种，$n-i$ 个数的（有序）方案数为 $b=(2^{m-1})^{n-i}$ 。

综合两部分，方案数为 $a*b*(C(n,i))$ 。$C(n,i)$ 表示序列的 $n$ 个位置中选取 $i$ 个用于顺序放置末位为 1 的 $i$ 个数。

## 参考程序
```cpp
#include<bits/stdc++.h>
using namespace std;

typedef long long ll;

const ll N=5005;
ll C[N][N]={{0}};
ll MOD=1e9+7;
ll n,m;

#define Get_Mod(x) (((x)+MOD)%MOD)
#define endl '\n'

void preC(){
    for(ll i=0;i<N;i++){
        C[i][0]=1;
        for(ll j=1;j<=i;j++){
            C[i][j]=(C[i-1][j]+C[i-1][j-1])%MOD;
        }
    }
}

ll qcpow(ll a,ll b,ll p){
    ll ret = 1;
    a%=p;
    while(b){
        if(b&1) ret = ret*a%p;
        a = a*a%p;
        b>>=1;
    }
    return ret;
}

signed main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr); cout.tie(nullptr);
    cin >> n >> m >> MOD;
    preC();
    ll t,a,b,ans=0;
    for(ll i=0;i<=n;i++){
        t = qcpow(2,i,MOD)-1;
        a = qcpow(t,m-1,MOD);
        t = qcpow(2,m-1,MOD);
        b = qcpow(t,n-i,MOD);
        t = Get_Mod(a*b);
        t = Get_Mod(C[n][i]*t);
        ans = Get_Mod(ans+t);
    }
    if(n==1) cout << Get_Mod(1) << endl;
    else if(m==1) cout << Get_Mod(ans-1) << endl;
    else cout << ans << endl;

    return 0;
}
```

# C.Sum of Suffix Sums

## 题目大意

给定一个初始为空的数组，你需要进行 `q` 次操作：

- 对于每次操作，给定两个非负整数 `t` 和 `v`，先从数组末尾取出 `t` 个元素，然后将 `v` 添加到数组末尾。保证 `t` 不会超过操作前数组的长度。

每次操作后，假设当前数组为 `a1, a2, ..., an`，计算 `s1, s2, ..., sn` 的总和，其中 `si = ai + ai+1 + ... + an` 是从位置 `i` 开始的后缀和。

由于结果可能非常大，输出时需对 `1000000007` 取模。

## 解题思路
考虑每个元素的贡献：第 $i$ 个元素对答案的贡献为 $i*a_i$ 。

在加入元素时，直接将其贡献加入答案。

同时对这个序列维护一个前缀和，以便快速移除元素和减去贡献。

## 参考程序
```cpp
const ll N = 200005;
void solve()
{
    ll q,t,rm;cin >> q;
    ll n=1;
    vector<ll> v,s;
    v.emplace_back(0);
    s.emplace_back(0);
    ll tm;
    ll sum=0;
    while(q--){
        cin >> rm >> t;
        sum = Get_Mod(sum-Get_Mod(s[n-1]-s[n-rm-1]));
        s.erase(s.end()-rm,s.end());
        v.erase(v.end()-rm,v.end());
        n-=rm;

        tm = t;
        tm = Get_Mod(tm*n); // 元素贡献
        sum = Get_Mod(sum+tm);
        v.emplace_back(tm);
        s.emplace_back(sum);
        n++;

        cout << sum << endl;
    }
}
```

# H.World Finals

## 题目大意
两场ICPC比赛，已知（预测的）两场比赛的所有队伍的解题数和罚时。

如果一支队伍同时具有两场比赛的资格，只能参加其中一场。

现在，`lzr010506`可以决定 同时具有两场比赛的资格 的队伍具体参加哪一场，根据预测数据求`lzr010506`可以得到的最高名次。

## 解题思路
开2个map分别记录两场比赛情况。

假设`lzr010506`参加其中一场，去掉那一场所有的 同时具有两场比赛的资格 的队伍，排序即可得到这一场的最佳名次。

如此求出两场比赛的最佳名次，取最小值即可。

## 参考程序
```cpp
const ll N = 200005;
string me="lzr010506";
bool cmp(const pair<string,pll> &a,const pair<string,pll> &b)
{
    pll pa,pb;
    pa=a.second; pb=b.second;
    if(pa.first==pb.first) return pa.second<pb.second;
    return pa.first>pb.first;
}
void solve()
{
    map<string,pll> v1,v2;
    ll n,m;
    cin >> n;
    string ts; ll t1,t2;
    ll ans1,ans2;ans1=ans2=0;
    FORLL(i,1,n)
    {
        cin >> ts >> t1 >> t2;
        v1[ts]=make_pair(t1,t2);
    }
    cin >> m;
    FORLL(i,1,m)
    {
        cin >> ts >> t1 >> t2;
        v2[ts]=make_pair(t1,t2);
    }
    map<string,pll> tmp;
    tmp = v1;
    for(auto &x:v2){
        if(tmp.count(x.first)&&x.first!=me){
            tmp.erase(x.first);
        }
    }
    auto tv = vector<pair<string,pll>>(tmp.begin(),tmp.end());
    sort(ALL(tv),cmp);
    for(auto &x:tv){
        ans1++;
        if(x.first==me) break;
    }
    tmp = v2;
    for(auto &x:v1){
        if(tmp.count(x.first)&&x.first!=me){
            tmp.erase(x.first);
        }
    }
    tv = vector<pair<string,pll>>(tmp.begin(),tmp.end());
    sort(ALL(tv),cmp);
    for(auto &x:tv){
        ans2++;
        if(x.first==me) break;
    }
    cout << min(ans1,ans2) << endl;
}
```

# I.Mirror Maze

**DFS**

## 题目大意
在一个 $n \times m$ 的镜子迷宫中，每个格子都有一面镜子。镜子的类型有以下四种：

1. `-`：来自上方或下方的光线将被反射回去，来自左方或右方的光线则继续前进而不会被反射。
2. `|`：来自左方或右方的光线将被反射回去，来自上方或下方的光线则继续前进而不会被反射。
3. `/`：来自左方、右方、上方、下方的光线将分别被反射到上方、下方、左方、右方。
4. `\`：来自左方、右方、上方、下方的光线将分别被反射到下方、上方、右方、左方。

现在有 $q$ 个光源（给定位置和方向）。光的信徒小G想知道，对于每个光源，在足够长的时间内，发出的光线会被反射经过的不同镜子的数量。

## 解题思路
对于一个光源，如果它的传播路径没有首尾相连成环，那么这个路径就只可能是一条链。
（证明：光的路径是可逆的，假设一条传播路径在某一点突然形成了环，那回溯时在这个点就可以有两条路径，这显然是不合理的）

对于给定的镜子阵列，我们可以直接处理出每个点向每个方向的答案，对最后的询问打表。

只要是一条链，它的最初起点和最后终点一定在阵列的边缘位置。我们可以从边缘位置开始向内DFS，直到遇到边缘位置，这样就可以得到一条链的路径，路径上的答案都可以处理得到。

没有遍历到的点，说明它们是环的一部分，我们可以通过时间戳来寻找和处理环上的答案。

## 参考程序
```cpp
//                D  U  R  L
const int dx[] = {1, -1, 0, 0};
const int dy[] = {0, 0, 1, -1};
const int c0[] = {1, 0, 2, 3}; // -
const int c1[] = {0, 1, 3, 2}; // |
const int c2[] = {3, 2, 1, 0}; // /
const int c3[] = {2, 3, 0, 1}; // \'

int conv(int d, char c){
    switch(c){
        case '-': return c0[d];
        case '|': return c1[d];
        case '/': return c2[d];
        case '\\': return c3[d];
    }
    return -1;
}

int n,m,q,tms,cnt; //tms:timestamp
vector<string> mp;
vector<vector<int>> vis;
vector<vector<array<int,4>>> ans,visd;
vector<tuple<int,int,int>> buf;

void dfs_line(int x,int y,int dir){
    buf.push_back({x,y,dir});
    if(x<0||x>=n||y<0||y>=m) return;
    int nd=conv(dir,mp[x][y]); //new direction
    dfs_line(x+dx[nd],y+dy[nd],nd);
}
void f_line(int sx,int sy,int sd){
    buf.clear();
    dfs_line(sx,sy,sd);
    reverse(buf.begin(),buf.end());
    int res=0; tms++;
    for(int i=0;i<buf.size()-1;i++){ //回溯
        auto [x,y,d]=buf[i];
        if(i){
            int fl=1;
            if(mp[x][y]=='-'&&(d&2)||mp[x][y]=='|'&&!(d&2)) fl=0; // 通过
            if(fl){
                if(vis[x][y]!=tms) res+=fl; // 反射
                vis[x][y]=tms;
            }
        }
        visd[x+dx[d^1]][y+dy[d^1]][d]=tms;
        ans [x+dx[d^1]][y+dy[d^1]][d]=res;
    }
}

void dfs_loop(int x,int y,int dir){
    visd[x][y][dir]=tms;
    x += dx[dir];
    y += dy[dir];
    int nd = conv(dir,mp[x][y]);
    if(nd!=dir){
        if(vis[x][y]!=tms) cnt++;
        vis[x][y]=tms;
    }
    if(visd[x][y][nd]!=tms) dfs_loop(x,y,nd);
}
void mk_loop(int x,int y,int dir){
    ans[x][y][dir] = cnt;
    visd[x][y][dir] = tms;
    x+=dx[dir];
    y+=dy[dir];
    int nd=conv(dir,mp[x][y]);
    if(visd[x][y][nd]!=tms) mk_loop(x,y,nd);
}
void f_loop(int sx,int sy,int sd){
    cnt=0; tms++;
    dfs_loop(sx,sy,sd);
    tms++;
    mk_loop(sx,sy,sd);
}

void solve()
{
    cin >> n >> m;
    mp.clear();
    mp.resize(n);
    for(auto& s:mp) cin >> s;
    ans.resize(n,vector<array<int,4>>(m,{0,0,0,0}));
    visd.resize(n,vector<array<int,4>>(m,{0,0,0,0}));
    vis.resize(n,vector<int>(m,0));
    for(int i=0;i<n;i++){
        f_line(i,0,2);
        f_line(i,m-1,3);
    }
    for(int i=0;i<m;i++){
        f_line(0,i,0);
        f_line(n-1,i,1);
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            for(int d=0;d<4;d++){
                if(visd[i][j][d]==0)
                    f_loop(i,j,d);
            }
        }
    }
    cin >> q;
    while(q--){
        int x,y,d;
        string s;
        cin >> x >> y >> s;
        x--,y--;
        if(s[0]=='b') d=0;
        if(s[0]=='a') d=1;
        if(s[0]=='r') d=2;
        if(s[0]=='l') d=3;
        cout << ans[x][y][d] << endl;
    }
}
```