---
title: 题解|2024牛客寒假06
tags:
  - 2024牛客寒假
categories: 算法竞赛
mathjax: true
abbrlink: ACM_2024Winter_NCD06
date: 2024/2/23
description: 题解|2024牛客寒假06
---

# A.宇宙的终结
**1签**
## 题目大意
在$[1,100]$中给定的某个区间内找到一个数，它是$3$个不同素数的积。

## 解题思路
打出$100$以内素数表，然后暴力枚举判定。

手算也行，$100$以内答案只有$30,42,66,70,78$。

## 参考程序
```cpp
vector<ll> phi={2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71,73,79,83,89,97};
bool pend(ll x){
    ll cnt=0;
    for(auto &p:phi){
        if(x%p==0) { cnt++; x/=p; }
        if(cnt>=3) break;
    }if(x==1&&cnt==3) return true;
    return false;
}
void solve()
{
    ll l,r;cin >> l >> r;
    FORLL(i,l,r) if(pend(i)) {cout << i << endl;return;} 
    cout << -1 << endl;
}
```

```cpp
vector<ll> ans={30,42,66,70,78};
void solve()
{
    ll l,r;cin >> l >> r;
    for(auto &x:ans) if(x>=l&&x<=r) {cout << x << endl;return;}
    cout << "-1" << endl;
}
```

# B.爱恨的纠葛
**思维**
## 题目大意
给定$2$个长度为$n$的数组$a,b$。调整$a$数组使得$|a_i-b_i|$的最小值最小。

## 解题思路
在$a,b$中分别找到一个数，它们的差的绝对值最小。

对数组$a$排序，在数组$a$中二分查找$b_i$的最近邻，记录差的绝对值最小的一对的下标$i,j$，最后把$a_i$和$a_j$交换使这一对在同一位置。

## 参考程序
```cpp
void solve()
{
    ll n,t;cin >> n;
    vector<ll> a(n),b(n);
    FORLL(i,0,n-1) cin >> a[i];
    FORLL(i,0,n-1) cin >> b[i];
    SORT(a);
    ll mindif=INF,idxa=-1,idxb=-1;
    FORLL(i,0,n-1)
    {
        auto it=upper_bound(ALL(a),b[i]);
        if(it!=a.end())
        {
            t=abs(b[i]-*it);
            if(t<mindif)
            {
                mindif=t;
                idxa=i;
                idxb=it-a.begin();
            }
        }
        if(it!=a.begin())
        {
            it--;
            t=abs(b[i]-*it);
            if(t<mindif)
            {
                mindif=t;
                idxa=i;
                idxb=it-a.begin();
            }
        }
    }
    swap(a[idxa],a[idxb]);
    FORLL(i,0,n-1) cout << a[i] << Presentation(i,n-1);
}
```

# C.心绪的解剖
**思维**
## 题目大意
给定一个数$x$，问$x$是否可以表示为$3$个斐波那契数之和。
其中，斐波那契数指的是斐波那契数列$0,1,1,2,3,5\cdots$ 中的数。

## 解题思路
预处理$N$以内斐波那契数列，每次二分找到恰小于等于$x$的数，然后$x$减去这个数。
三次之后，如果$x$为$0$，则可以表示。

## 参考程序
```cpp
void preFeb(){
    Feb.pb(0);
    Feb.pb(1);
    while(Feb.back()<=1e10){
        Feb.pb(Feb[Feb.size()-1]+Feb[Feb.size()-2]);
    }
}
void solve()
{
    ll x;cin >> x;
    vector<ll> ans(3,0);
    FORLL(i,0,2){
        auto it = --upper_bound(ALL(Feb),x);
        ans[i] = *it;
        x -= *it;
    }
    if(x) cout << -1 << endl;
    else cout << ans[0] << ' ' << ans[1] << ' ' << ans[2] << endl;
}
```

# D.友谊的套路
**2签**
## 题目大意
在五局三胜的游戏中，一方先胜$2$场，另一方再胜$3$场逆袭，这种情况叫做“让二追三”。
已知小红的胜率为$p$，求让二追三的概率。

## 解题思路
分小红让二追三和小红被让二追三两种情况。

## 参考程序
```cpp
void solve()
{
    ld p;cin >> p;
    ld np=1-p;
    ld ans=p*p*np*np*np+np*np*p*p*p;
    cout << fixed << setprecision(10) << ans << endl;
}
```

# E.未来的预言
**思维**
## 题目大意
给定一个形如“BOx”的字符串，表示进行$x$局的游戏（$x$为奇数），赢得更多场次的队伍获胜。
另给一个字符串，表示每局胜利的队伍。

问第几局已经确定了胜负，或是还未确定。

## 解题思路
数数

## 参考程序
```cpp
ll getnum(string s){
    ll ret=0;
    FORLL(i,2,s.length()-1){
        ret*=10;
        ret+=s[i]-'0';
    }
    return ret;
}
void solve()
{
    string ts;cin >> ts;
    string s; cin >> s;
    ll tn=getnum(ts); tn/=2;
    ll cntR=0,cntP=0;
    ll n=s.length();
    FORLL(i,0,n-1){
        char c=s[i];
        if(c=='R') cntR++;
        else cntP++;
        if(cntR>tn){
            cout << "kou!" << endl << i+1 << endl;
            return;
        }else if(cntP>tn){
            cout << "yukari!" << endl << i+1 << endl;
            return;
        }
    }
    cout << "to be continued." << endl << n << endl;
}
```

# F.命运的抉择
**思维**
## 题目大意
给定一个长度为$n$的数组，把它分成非空的两部分$a,b$，使得从$a,b$中分别任取一个元素$a_i,b_j$，它们互质$gcd(a_i,b_j)=1$

## 解题思路
先在$a$中放置一个数，开一个set记录$a$中已存在的质数因子。

在剩余的数字中多次扫描，一旦某个数$x$能被$a$中的质数因子整除，就把它放入$a$中，直到某次扫描没有加入新的数字。
剩余的数字作为$b$。

时间复杂度比较抽象//

## 参考程序
```cpp
vector<ll> GetFac(ll x){//返回x的质因数集合
    vector<ll> ret;
    ll i=0;
    while(x>1){
        if(check[x]){//check[i]==1表示i是合数
            if(x%prime[i]==0){//prime是质数表
                ret.pb(prime[i]);
                while(x%prime[i]==0) x/=prime[i];
            }
        }else{ ret.pb(x); return ret; }
        i++;
    }return ret;
}
void solve()
{
    ll n,t;cin >> n;
    vector<vector<ll>> vFac;
    vector<ll> v;
    FORLL(i,1,n){
        cin >> t;
        v.push_back(t);
        vFac.push_back(GetFac(t));
    }
    set<ll> st;
    vector<int> sa(n,0);//放入集合a
    sa[0]=1; t=1;
    for(auto &x:vFac[0]) st.insert(x);//将第一个数的质因数放入集合
    while(t){
        t=0;//t表示本轮是否有数放入集合a
        FORLL(i,0,n-1) if(sa[i]==0){
            for(auto &x:vFac[i]) if(st.count(x)){
                sa[i]=1; t++;
                for(auto &y:vFac[i]) st.insert(y);
                break;
            }
        }
    }
    vector<ll> a,b;
    FORLL(i,0,n-1) if(sa[i]) a.pb(v[i]); else b.pb(v[i]);
    if(b.size()==0) {cout << "-1 -1" << endl;return;}
    ll na=a.size(),nb=b.size();
    cout << na << ' ' << nb << endl;
    FORLL(i,0,na-1) cout << a[i] << Presentation(i,na-1);
    FORLL(i,0,nb-1) cout << b[i] << Presentation(i,nb-1);
}
```

# G.人生的起落
**构造**
## 题目大意
形如$(a,b,a),a\gt b$的三元组称为“v-三元组”。
构造一个长度为$n$，和为$S$，且恰好有$k$个“v-三元组”的正整数数组。

## 解题思路
限定正整数，“v-三元组”至少为$(2,1,2)$。
依此开局特判：
1. $n<k*2+1$：k个“v-三元组”最少需要$2k+1$个数。
2. $S<n+k+1$：k个“v-三元组”最少需要$k+1$个$2$，其余为$1$，和至少为$n+k+1$。

构造方法是，先在最前的$k+1$个奇数位上放置尽可能大的数，然后在剩余的位置上放置$1$。若还有多余的数，放在第$k+1$个奇数位（$k*2+1$）上。

若$n$恰好等于$k*2+1$，则将多余的数放置在偶数位上，但偶数位上的数不能超过奇数位上的数。

## 参考程序
```cpp
void solve()
{
    ll n,S,k;cin >> n >> S >> k;
    if(n==1){
        if(k==0) cout << S << endl;
        else cout << "-1\n";
        return ;
    }
    if(n==2){
        if(k==0) cout << S/2 << " " << S-S/2 << endl;
        else cout << "-1\n";
        return ;
    }
    if(k==0){
        FORLL(i,1,n-1) cout << "1" << ' ';
        cout << S-n+1 << endl;
        return ;
    }
    if(n<2*k+1) {cout << "-1\n";return ;}
    if(k&&S<n+k+1) {cout << "-1\n";return ;}
    ll t=(S-(n-(k+1)))/(k+1);
    vector<ll> ans;
    FORLL(i,1,k){
        ans.pb(t);
        ans.pb(1);
        S-=t+1;
    }ans.pb(t); S-=t;
    if(ans.size()>=n){
        t=S/(n/2);
        FORLL(i,0,n/2-1){
            ans[i*2+1]+=t;
            S-=t;
        }
        t=0;
        while(S){
            ans[t*2+1]++;
            S--;
            t++;
        }
        if(n>1&&ans[1]>=ans[0]) {cout << "-1\n";return ;}
    }else{
        t=ans.size();
        while(ans.size()<n){
            ans.pb(1);
            S--;
        }
        ans[t]+=S;
    }
    FORLL(i,0,n-1) cout << ans[i] << " \n"[i==n-1];
}
```

# I.时空的交织
**贪心**
## 题目大意
给定$n$个数的数组$a$和$m$个数的数组$b$，表示一个$n\times m$的矩阵，矩阵中的元素$m_{i,j}=a_i*b_j$。
选择一个子矩阵，使得子矩阵中的元素之和最大，输出这个和。

## 解题思路
假设选定的矩阵区间为$(r_i,r_j):(c_i,c_j)$，则子矩阵的和为$\sum_{i=r_i}^{r_j}\sum_{j=c_i}^{c_j}a_i*b_j=\sum_{i=r_i}^{r_j}a_i*\sum_{j=c_i}^{c_j}b_j$。

问题转化为求$a$数组的一个非空连续子数组和乘上$b$数组的一个非空连续子数组和的最大值。

可为空的数组区间和的最大值可以贪心的求解：
1. 用$cur$表示对当前区间和，$max$表示区间和的最大值。
2. 将当前元素$a_i$加入区间，和当前最大值比较。
3. 若当前区间和$cur<0$，则舍弃之前的区间，重新开始计算（$cur:=0$）。

本题中的区间要求非空，可以用最值修正。

此外，$a$数组和$b$数组的元素可以为负数，因此同时求出区间和的最大值和最小值，两两相乘取最大即可。

## 参考程序
```cpp
void solve()
{
    ll n,m;cin >> n >> m;
    create_vec(a,n);
    create_vec(b,m);
    ll mxa,mxb,mna,mnb,curmn,curmx;
    mxa=mxb=-INF; mna=mnb=INF;
    curmn=curmx=0;
    FORLL(i,0,n-1){
        curmn+=a[i];
        curmx+=a[i];
        if(curmn>0) curmn=0;
        if(curmx<0) curmx=0;
        mna=min(mna,curmn);
        mxa=max(mxa,curmx);
    }
    curmn=curmx=0;
    FORLL(i,0,m-1){
        curmn+=b[i];
        curmx+=b[i];
        if(curmn>0) curmn=0;
        if(curmx<0) curmx=0;
        mnb=min(mnb,curmn);
        mxb=max(mxb,curmx);
    }
    curmn=INF; curmx=-INF;
    FORLL(i,0,n-1){
        curmn=min(curmn,a[i]);
        curmx=max(curmx,a[i]);
    }
    if(curmx<=0) mxa=min(mxa,curmx);
    if(curmn>=0) mna=max(mna,curmn);
    curmn=INF; curmx=-INF;
    FORLL(i,0,m-1){
        curmn=min(curmn,b[i]);
        curmx=max(curmx,b[i]);
    }
    if(curmx<=0) mxb=min(mxb,curmx);
    if(curmn>=0) mnb=max(mnb,curmn);
    cout << max({mna*mnb,mna*mxb,mxa*mnb,mxa*mxb}) << endl;
}
```

# J.时空的交织
**贪心、构造**
## 题目大意
给定一棵有根树，若干个节点为红色。
为每个节点赋值$1$或$2$，使得每个以红色节点为根的子树，其节点值之和为$3$的倍数。

## 解题思路
对于每个红色节点，如果它没有白色子节点，则它的子树除它以外的和已经是$3$的倍数，它为$1$或$2$都不可能再使它的子树和为$3$的倍数。

如果它至少有1个白色子节点，则它和白色子节点可以配合使得它的子树和为$3$的倍数。

因此，按DFS逆序遍历，白色节点先赋值为$1$。若红色节点除其本身外，和不是$3$的倍数，则用它补上；否则将任一白色子节点改为$2$，它赋为$2$。

## 参考程序
```cpp
vector<vector<ll>> G;
vector<ll> vDFS_rev;
void DFS(ll x)
{
    for(auto &y:G[x]) DFS(y);
    vDFS_rev.pb(x);
}
void solve()
{
    ll n,t;cin >> n; G.resize(n+1);
    string s;cin >> s;
    bitset<N> red;//red[i]表示第i个节点是否为红色
    FORLL(i,0,n-1) red[i+1]=(s[i]=='R');
    vector<ll> white(n+1,0);//white[i]表示第i个节点下的某一白色节点，0表示没有
    FORLL(i,2,n){
        cin >> t; G[t].push_back(i);
        if(white[t]==0&&!red[i]) white[t]=i;
    }
    FORLL(i,1,n) if(white[i]==0&&red[i]) {cout << "-1\n";return ;}//红色节点下必须有白色节点
    DFS(1);//获取逆DFS序列
    ll S[N]={0},ans[N]={0};
    for(auto &x:vDFS_rev){
        for(auto &y:G[x]) S[x]+=S[y];
        if(red[x]){
            if(S[x]%3==0){
                ans[x]=2; S[x]+=3;
                ans[white[x]]=2; S[white[x]]+=1;
            }else if(S[x]%3==1){
                ans[x]=2; S[x]+=2;
            }else{
                ans[x]=1; S[x]+=1;
            }
        }else{
            ans[x]=1;
            S[x]+=1;
        }
    }
    FORLL(i,1,n) cout << ans[i];
    cout << endl;
}
```