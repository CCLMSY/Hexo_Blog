---
title: 题解|2024暑期牛客多校10
date: 2024/8/15
updated:
tags:
  - 2024暑期牛客多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_NCD10
description: 题解|2024暑期牛客多校10
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

比赛链接：[2024牛客暑期多校训练营10](https://ac.nowcoder.com/acm/contest/81605)

# A.Surrender to My Will
## 题意
给定一个长度为 $5$ 的字符串，`Y`代表投降，`N`代表不投降，`-`代表未投票。
投降人数不小于4人即可投降。
问字符串表示的投降结果。

## 解题思路
计数判断

## 参考代码
```cpp
void solve()
{
    string s;cin >> s;
    map<char,int> mp;
    for(auto c:s) mp[c]++;
    if(mp['Y']>=4) {cout << 1 << endl;return;}
    if(mp['N']>=2) {cout << -1 << endl;return;}
    cout << 0 << endl;
}
```

# B.std::pair
## 题意
基础合法类型`int`和`double`。
对于任意2个合法类型`A`和`B`，`pair<A,B>`是合法的，可以通过`.first`和`.second`分别访问`A`和`B`成员。
声明变量的格式为`pair<A,B> 变量名;`。

给定 $n$ 个字符串表示变量声明，变量名不重复。
再给定 $q$ 个字符串表示访问变量，返回被查询的成员类型。

## 解题思路
模拟题，对每个变量类型构建二叉树，查询在二叉树上进行。
具体实现方式可以参考代码。

## 参考代码
```cpp
pll v[N]; //pair的树，x表示左子树，y表示右子树
map<string,ll> dict; //变量二叉树的根所在下标
ll cnt=0;
pll parse_type(string s){ //解析类型，构建树
    if(s=="int") return {-1,-1}; //int
    if(s=="double") return {-2,-2}; //double
    s=s.substr(5,s.length()-6); 
    ll cntp=0,cntd=0,pos;
    FORLL(i,0,s.length()-1){ //找到当前pair对应的逗号位置pos
        if(s[i]=='p') cntp++;
        if(s[i]==','){
            if(cntp==cntd) {pos=i;break;} 
            cntd++;
        }
    }
    string ltype=s.substr(0,pos),rtype=s.substr(pos+1,s.length()-pos-1);
    ll lidx=cnt++,ridx=cnt++;
    v[lidx]=parse_type(ltype);
    v[ridx]=parse_type(rtype);
    return make_pair(lidx,ridx);
}
string get_type(ll idx){ //重组类型字符串
    if(v[idx].x==-1) return "int";
    if(v[idx].x==-2) return "double";
    return "pair<"+get_type(v[idx].x)+","+get_type(v[idx].y)+">";
}
void solve()
{
    ll n,q;cin >> n >> q;
    string type,name;
    FORLL(i,1,n){
        cin >> type >> name;
        name.pop_back();
        dict[name]=cnt++;
        v[dict[name]]=parse_type(type);
    }
    FORLL(i,1,q){
        cin >> name;
        ll j=0;
        while(j<name.length()&&name[j]!='.') j++;
        string main_name=name.substr(0,j); j++;
        ll idx=dict[main_name];
        while(j<name.length()){
            if(name[j]=='f'){ //first
                idx=v[idx].x;
                j+=6;
            }else if(name[j]=='s'){ //second
                idx=v[idx].y;
                j+=7;
            }
        }
        cout << get_type(idx) << endl;
    }
}
```

# F.Collinear Exception
## 题意
有一 $n\times n$ 的点阵列，每个点的坐标为 $(i,j)$ ，$1\leq i,j\leq n$ 。
按给定序列对点进行标记，要求标记后不能存在三个被标记点共线，否则标记失败。
按给定序列顺序输出每个点是否标记成功。

## 解题思路
每当加入一个新的点时，和已有的点连线，标记线上的所有点。
被标记的点不能再被加入。

## 参考代码
```cpp
void solve()
{
    ll n;cin >> n;
    vector<vector<int>> vis(n+1,vector<int>(n+1,0));
    vector<pll> points;
    ll ub=n*n,xx,yy,ans=0;
    FORLL(i,1,ub)
    {
        cin >> xx >> yy;
        if(vis[xx][yy]){
            cout << '0';
            continue;
        }
        for(auto[tx,ty]:points){ //标记线上的点
            if(xx==tx){
                FORLL(j,1,n) vis[xx][j]=1;
            }else{
                ll dx=xx-tx,dy=yy-ty;
                ll g=__gcd(dx,dy);
                dx/=g;dy/=g;
                ll ttx=tx,tty=ty;
                while(ttx<=n&&ttx>=1&&tty<=n&&tty>=1){
                    vis[ttx][tty]=1;
                    ttx+=dx; tty+=dy;
                }
                ttx=tx; tty=ty;
                while(ttx<=n&&ttx>=1&&tty<=n&&tty>=1){
                    vis[ttx][tty]=1;
                    ttx-=dx; tty-=dy;
                }
            }
        }
        points.emplace_back(xx,yy);
        vis[xx][yy]=1;
        cout << '1';
    }cout << endl;
}
```

# H.All-in at the Pre-flop
## 题意
两名玩家初始分别有 $a,b$ 的筹码，每轮游戏各有 $\frac{1}{2}$ 的概率获胜。
假设当前轮两名玩家分别有 $x,y$ 的筹码，那么输家需要付给赢家 $\min(x,y)$ 的筹码。
问两名玩家获胜的概率。

## 解题思路
打个表，观察到答案是 $\frac{a}{a+b},\frac{b}{a+b}$ 。

## 参考代码
```cpp
void solve()
{
    ll a,b;cin >> a >> b;
    ll c=inv(add(a,b));
    cout << mul(a,c) << ' ' << mul(b,c) << endl;
}
```