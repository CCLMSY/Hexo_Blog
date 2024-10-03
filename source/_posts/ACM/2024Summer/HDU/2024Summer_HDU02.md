---
title: 题解|2024暑期杭电多校02
date: 2024/7/22
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU02
description: 题解|2024暑期杭电多校02
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（2）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A82%A3%A9&source=1&searchmode=source)

# (1001)HDU7445.鸡爪
**构造**
## 题意
一个鸡爪由一个中心节点和3条（不含端点的）边构成。如下图中，三条边和节点 $1$ 构成一个鸡爪，节点 $2,3,4$ 不属于这个鸡爪。
![image](/images/ACM//2024Summer_HDU02_1001_1.png)

给定正整数 $n$ ，允许使用 $n$ 条边和任意个节点。要求构造一张图，在包含尽可能最多的鸡爪的条件下，按顺序输出图的 $n$ 条边时的字典序最小。

## 解题思路
不限制节点数的情况下，想要构造最多$n/3$（整除）个的鸡爪是很容易的，问题的关键是如何构造使得字典序最小。

先讨论 $n$ 是3的倍数的情况。
由于一个中心点只能被一个鸡爪使用，贪心的想法便是让节点 $1$~$n/3$ 成为中心点，且依次尽量让节点 $1,2,3$ 连接更多的点。

$4$~$n/3$ 这些点首先作为中心点和 $1,2,3$ 相连。
$1,2,3$ 三个点互相连接后，无法构成以 $1,2,3$ 为中心的鸡爪，因此需要借助 $n/3+1, n/3+2, n/3+3$ 这三个点进行构造。

连接图如下：
![image](/images/ACM/2024Summer_HDU02_1001_2.png)

其他情况：
对于 $n$ 不是3的倍数的情况，将多余的1或2个点连接到节点1上。
$n\le 6$时，由于节点数太少，需要单独构造。

## 参考程序
```cpp
vector<vector<pll>> ans={
    {}, // 0
    {{1,2}},
    {{1,2},{1,3}},
    {{1,2},{1,3},{1,4}}, // 1
    {{1,2},{1,3},{1,4},{1,5}},
    {{1,2},{1,3},{1,4},{1,5},{1,6}},
    {{1,2},{1,3},{1,4},{1,5},{2,3},{2,4}}, // 2
};
void solve()
{
    ll n;cin >> n;
    if(n<=6){
        for(auto p:ans[n]){
            cout << p.first << ' ' << p.second << endl;
        }return ;
    }
    vector<pll> edges;
    ll cnt = n/3;
    FORLL(i,2,cnt+3) edges.emplace_back(1,i); 
    FORLL(i,3,cnt+2) edges.emplace_back(2,i);
    FORLL(i,4,cnt+1) edges.emplace_back(3,i); 
    FORLL(i,1,n%3) edges.emplace_back(1,cnt+3+i);
    SORT(edges);
    for(auto p:edges){
        cout << p.first << ' ' << p.second << endl;
    }
    // cout << "Test:" << edges.size() << endl;
}
```

# (1003)HDU7447.绝对不模拟的简单魔方
## 题意
给定一个三阶魔方，用$1$~$6$表示6个面的颜色。
魔方从初始还原状态起，侧面被扭转了不超过3次，且有一个角上的两片颜色贴纸可能贴错。
确定魔方的贴纸是否被贴错。如果有贴错，输出贴错那个角的三个颜色。

## 解题思路
扭转三次的操作对于魔方的角来说没有任何影响，只是虚晃一枪（题目中也提示了“绝对不模拟”）。
>但是赛时还是有神速度写模拟而且一遍过了%%%%

只需要先把正常状态的魔方的8个角按同一顺序（如顺时针）记录下来，统一以$1,6$开头以唯一确定三元组。
再把输入的魔方的8个角按同一顺序（顺时针）记录下来，统一以$1,6$开头，然后在正常状态的记录中查找，找不到的就是被贴错贴纸的角。

## 参考程序
```cpp
struct cor{
    int a,b,c;
    cor(int _a,int _b,int _c):a(_a),b(_b),c(_c){}
    bool operator==(cor t)const{
        return a==t.a&&b==t.b&&c==t.c;
    }
    void srt(){ //按大小排序
        if(a>b) swap(a,b);
        if(b>c) swap(b,c);
        if(a>b) swap(a,b);
    }
    void mkstd(){ //把三元组化为以1或6开头的形式
        int t;
        while(a!=1&&a!=6){
            t=a; a=b; b=c; c=t;
        }
    }
};
//原始魔方八个角，顺时针
vector<cor> std_cor = {{1,2,3},{1,3,4},{1,4,5},{1,5,2},{6,3,2},{6,4,3},{6,5,4},{6,2,5}};
void solve()
{
    vector<string> mf(9);
    for(auto& s:mf) cin >> s;
    vector<cor> corners={//给定魔方的八个角，顺时针
        {mf[0][3]-'0',mf[3][11]-'0',mf[3][0]-'0'},
        {mf[8][3]-'0',mf[5][0]-'0',mf[5][11]-'0'},
        {mf[3][8]-'0',mf[3][9]-'0',mf[0][5]-'0'},
        {mf[5][8]-'0',mf[8][5]-'0',mf[5][9]-'0'},
        {mf[6][3]-'0',mf[5][3]-'0',mf[5][2]-'0'},
        {mf[3][3]-'0',mf[2][3]-'0',mf[3][2]-'0'},
        {mf[6][5]-'0',mf[5][6]-'0',mf[5][5]-'0'},
        {mf[3][6]-'0',mf[2][5]-'0',mf[3][5]-'0'}};
    for(auto& cor:corners) cor.mkstd();
    for(auto& cor:corners){
        int fl=0;
        for(auto& st:std_cor) if(st==cor) {fl=1;break;}
        if(fl) continue;
        cor.srt();
        cout << cor.a << ' ' << cor.b << ' ' << cor.c << endl;
        return;
    }
    cout << "No problem" << endl;
}
```

# (1006)HDU7450.传奇勇士小凯
## 题意
一棵以 $1$ 为根， $n$ 个节点的树，在第 $i$ 个节点处，每天有 $\dfrac{p_i}{15}$ 的概率可以离开当前节点，并选择一个子节点走一步。
从根节点 $1$ 走到任意叶子节点的最大天数的期望。

## 解题思路
在第 $i$ 个节点处，记停留在该节点的期望天数为 $x$ ，有 $\dfrac{p_i}{15}$ 的概率可以离开（停留 $1$ 天），有 $1-\dfrac{p_i}{15}$ 的概率停留在当前节点（停留 $x+1$ 天）。
有 $x = \dfrac{p_i}{15} + \dfrac{15-p_i}{15} \cdot (1+x)$ 成立，解得 $x = \dfrac{15}{p_i}$ 。

问题转换为选择一条以节点 $1$ 开头，以叶子节点结尾的路径，使得路径上的每个节点的期望天数 $\dfrac{15}{p_i}$ 之和最大。

## 参考程序
```cpp
void form(pll& p){ //化简
    ll g = __gcd(p.first,p.second);
    p.first/=g;
    p.second/=g;
}
inline bool smaller(pll a,pll b){ //a<b
    return 1.*a.first/a.second<1.*b.first/b.second;
}
void addto(pll& a,pll b){ //a+=b
    a.first=a.first*b.second+a.second*b.first;
    a.second*=b.second;
    form(a);
}
vector<ll> vdfs; //dfs序列
vector<vector<ll>> G;
vector<ll> fa;
vector<pll> ans;
void dfs(ll x,ll f){
    fa[x]=f;
    vdfs.emplace_back(x);
    for(auto &y:G[x]) if(y!=f) dfs(y,x);
}
void solve()
{
    ll n;cin >> n;
    G.clear(); G.resize(n+1);
    fa.clear(); fa.resize(n+1);
    ans.clear(); ans.resize(n+1,{0,1});
    vdfs.clear();
    ll u,v,t;
    FORLL(i,1,n-1){
        cin >> u >> v;
        G[u].emplace_back(v);
        G[v].emplace_back(u);
    } dfs(1,0);
    FORLL(i,1,n){
        cin >> t;
        ans[i]={15,t}; //直接存入第i个节点的期望
    }
    reverse(ALL(vdfs));
    for(auto cur:vdfs){ //逆DFS序遍历
        pll mx={0,1};
        for(auto &y:G[cur]) if(y!=fa[cur]){
            if(smaller(mx,ans[y])) mx=ans[y];
        }
        addto(ans[cur],mx);
    }
    cout << ans[1].first << '/' << ans[1].second << endl;
}
```

# (1007)HDU7451.URL划分
## 题意
给定字符串URL，形如：`s3://hdu-oj-bucket/problem=1/type=data/`
从中提取出：
- 传输协议：`://`之前的内容，如`s3`
- 网络位置：`://`之后，第一个`/`之前的内容，如`hdu-oj-bucket`
- 环境变量：由`/`分割，形如`A=B`的内容，如`problem=1`，`type=data`

## 解题思路
按题目要求提取即可

## 参考程序
```cpp
void solve()
{
    string s; cin >> s;
    string protocol;
    string location;
    vector<string> var;
    string varname,val;
    ll n=s.length(),i=0,t;
    for(i=0;i<n;i++){
        if(s[i]==':'){
            protocol = s.substr(0,i);
            break;
        }
        t = 1;
    } i+=3;
    ll j=i;
    for(;i<n;i++){
        if(s[i]=='/'){
            location = s.substr(j,i-j);
            break;
        }
    } i++;
    while(i<n){ //匹配varname=val，以/分隔
        j=i;
        while(j<n&&s[j]!='='&&s[j]!='/') j++;
        if(s[j]=='/'){ //跳过
            i=j+1;
            continue;
        }else if(s[j]=='='){
            varname=s.substr(i,j-i);
            i=j+1; j=i;
            while(j<n&&s[j]!='/') j++;
            val=s.substr(i,j-i);
            var.emplace_back(varname+"="+val);
        }
    }
    cout << protocol << endl;
    cout << location << endl;
    for(auto &x:var) cout << x << endl;
}
```
> 写完python才发现hduOJ不支持python...
```python
import re

T = int(input())

for _ in range(T):
    url = input()
    protocol, location, *path = re.split(r"://|/", url)
    variables = [p for p in path if "=" in p]
    print(protocol)
    print(location)
    for variable in variables:
        print(variable)
```

# (1010)HDU7454.女神的睿智
## 题意
给定长度为$8$的字符串，由`R,G,B`三种字符组成。
进行以下三轮操作，每轮操作从左往右两两合并：
1. 颜色相同时，合并为该颜色；颜色不同时，合并为左侧颜色
2. 颜色相同时，合并为该颜色；颜色不同时，合并为左侧颜色
3. 颜色相同时，合并为该颜色；颜色不同时，比较两种颜色在初始字符串中的数量，合并为数量多的一种；数量相同时，合并为随机颜色

求最终合并后的颜色：`R,G,B`或不确定颜色`N`

## 解题思路
按题目要求模拟即可

## 参考程序
```cpp
void solve()
{
    string s,s2; cin >> s;
    map<char,int> mp;
    for(auto c:s) mp[c]++;
    for(ll i=0;i<8;i+=2) s2.push_back(s[i]);
    s.clear();
    for(ll i=0;i<4;i+=2) s.push_back(s2[i]);
    if(s[0]==s[1]) cout << s[0] << endl;
    else if(mp[s[0]]!=mp[s[1]]) cout << s[mp[s[0]]<mp[s[1]]] << endl;
    else cout << 'N' << endl;
}
```

# (1011)HDU7455.在 A 里面找有 C 的 B

//TODO