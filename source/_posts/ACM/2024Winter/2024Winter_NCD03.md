---
title: 题解|2024牛客寒假03
tags:
  - 2024牛客寒假
categories: 算法竞赛
mathjax: true
abbrlink: 87bdfd2f
date: 2024/2/7
description: 题解|2024牛客寒假03
---
# A.智乃与瞩目狸猫、幸运水母、月宫龙虾
**1签**
## 题目大意
题目分享了一个冷知识：
Ubuntu 发行版的每个代号都包含一个形容词和一个动物。例如：瞩目狸猫(Focal Fossa)、幸运水母(Jammy Jellyfish)、月宫龙虾(Lunar Lobster)，每个代号的两个单词首字母相同。
现在给定两个字符串，问首字母是否相同（忽略大小写）。

## 解题思路
直接比较首字母即可。

## 参考程序
```cpp
void solve()
{
    string s1,s2;cin >> s1 >> s2;
    if(s1[0]==s2[0]||s1[0]-32==s2[0]||s1[0]+32==s2[0]) cout << YES;
    else cout << NO;
}
```

# B.智乃的数字手串
**博弈、猜结论**
## 题目大意
有一串首尾相连的数字，两个玩家轮流操作。
当且仅当相邻$2$个数字之和为偶数时，可以消除其中一个，
然后**可以**交换剩下的数字中任意两个数字的位置（也可以不交换）。
特别的，如果只有$1$个数字，可以直接消除。
最先无法操作的玩家输。
问对于给定的数字串，qcjj（先手）和zn（后手）谁会赢。

## 解题思路
考虑游戏结束时的状态：手串上的数字排布为`奇偶奇偶...奇偶`，特点是数量为偶数。
数量为奇数时，一定存在两个相邻的数，奇偶校验相同（和为偶数）。

判断原始数字串长度的奇偶性即可。

## 参考程序
```cpp
void solve()
{
    ll n;cin >> n;
    create_vec(v,n);
    if(n&1) cout << "qcjj" << endl;
    else cout << "zn" << endl;
}
```

# D.chino's bubble sort and maximum subarray sum(easy version)
**最大子段和**
## 题目大意
给定一个长度为$n$的数组$a$，求恰好执行“交换任意相邻元素”操作$k$次后，数组$a$的最大非空子段和。

## 解题思路
由于easy version的$k$范围极小（$k\in [0,1]$），暴力枚举所有情况求最大非空子段和即可。

对于最大非空字段和的求法，可以用贪心方法：
记$cur$为：包含当前位置元素的最大子段和。
从$a_2$开始遍历数组，记当前元素为$a_i$，则当前最大子段和有$2$种情况：
1. 将当前位置的元素加入当前最大子段和，值为$cur+a_i$；
2. 以当前位置的元素为起点，重新开始计算最大子段和，值为$a_i$。

每个位置的$cur$取这$2$种情况的较大值，每个位置$cur$的最大值即为最大非空子段和。

## 参考程序
```cpp
void solve()
{
    ll n,k;cin >> n >> k;
    create_vec(v,n);
    ll ans=-INF,cur;
    if(k==1){
        vector<ll> tv;
        FORLL(i,0,n-2){
            tv=v;
            swap(tv[i],tv[i+1]);
            ll tans=tv[0];
            cur=tv[0];
            FORLL(j,1,n-1){
                cur=max(tv[j],cur+tv[j]);
                tans=max(tans,cur);
            }ans=max(ans,tans);
        }
    }else{//求原数组的最大子段和
        ans=v[0];
        cur=v[0];
        FORLL(i,1,n-1){
            cur=max(v[i],cur+v[i]);
            ans=max(ans,cur);
        }
    }
    cout << ans << endl;
}
```

# GH.智乃的比较函数(easy+normal)
**偏序**
## 题目大意
给定$3$个数字的$n$对关系`x y z`，$z=1$表示$x\lt y$；$z=0$表示$x\ge y$。
问这$n$对关系是否存在逻辑矛盾。

## 解题思路
由于只有$3$个数字，可以直接暴力赋值，验证是否有情况可以满足所有关系。
时间复杂度：$O(3^3n)$

赛时本人直接对给定的关系集进行判断，会很麻烦，需要注意很多细节；
还有大佬用Floyd算法求偏序关系//%%%

## 参考程序
```cpp
vector<array<ll,3>> cmp;
int pending(array<ll,4> a){
    for(auto [x,y,o]:cmp){
        if(o==1) { if(a[x]>=a[y]) return 0; }
        else { if(a[x]<a[y]) return 0; }
    }return 1;
}//判断是否满足所有关系
void solve()
{
    ll n,x,y,o;
    cin >> n;
    cmp.clear();
    FORLL(i,1,n){
        cin >> x >> y >> o;
        cmp.pb({x,y,o});
    }
    int ans=0;
    FORLL(i,1,3) FORLL(j,1,3) FORLL(k,1,3) ans|=pending({0,i,j,k});
    //存在满足所有关系的情况即可
    cout << (ans?YES:NO);
}
```

# J.智乃的相亲活动
**建图、概率论**
## 题目大意
有$n$名男嘉宾和$m$名女嘉宾，异性之间存在$k$对双向的好感关系。
每位男嘉宾和女嘉宾都会从ta喜欢的异性中均匀随机的选择一个，
被至少选中一次的嘉宾称为“心动嘉宾”。
求本次相亲活动中，心动男嘉宾和女嘉宾的期望数量。

## 解题思路
建图：对每对好感关系，建立一条从男嘉宾到女嘉宾的无向边。
对于任一嘉宾$i$：
ta喜欢的每位嘉宾被ta选中的概率为$\frac{1}{deg_i}$；
ta成为心动嘉宾的概率为$1-\prod_{j\in like_i}(1-\frac{1}{deg_j})$，其中$like_i$为ta喜欢的嘉宾集合（因为好感关系为双向）。

分别求出每位嘉宾成为心动嘉宾的概率，再对男女嘉宾分别求和即可。

## 参考程序
```cpp
void solve()
{
    ll n,m,k;cin >> n >> m >> k;
    vector<vector<ll>> G(n+m+1);
    vector<ll> deg(n+m+1);
    ll u,v;
    FORLL(i,1,k)
    {
        cin >> u >> v;  
        G[u].pb(n+v);
        G[n+v].pb(u);
        deg[u]++;
        deg[n+v]++;
    }
    ll ansn,ansm,t;
    ansn=ansm=0;
    FORLL(i,1,n){
        t=1;
        for(auto &x:G[i]){
            multo(t,mdiv(deg[x]-1,deg[x]));
        }
        addto(ansn,sub(1,t));
    }
    FORLL(i,n+1,n+m){
        t=1;
        for(auto &x:G[i]){
            multo(t,mdiv(deg[x]-1,deg[x]));
        }
        addto(ansm,sub(1,t));
    }
    cout << "modint" << endl;
    cout << ansn << ' ' << ansm << endl;
}
```

# K.智乃的“黑红树”
**数据结构**
## 题目大意
定义“黑红树”满足以下条件：
1. 一棵以黑色节点$1$为根的二叉树
2. 每个节点只能有$0$或$2$个子节点。
3. 黑色节点的子节点只能是红色；红色节点的子节点只能是黑色。

给定黑红节点的数量$a,b$，构造一棵黑红树。

## 解题思路
根据定义$1,2$，黑色节点的个数必须为奇数，红色节点的个数必须为偶数，因此$a$必须为奇数，$b$必须为偶数。
根据定义$3$，节点颜色将由节点的深度决定。

在满二叉树的情况下：
1. 如果最后一层是黑色节点，则节点数满足$a=2*b+1$
2. 如果最后一层是红色节点，则节点数满足$b=2*a$

利用这两个关系，可以再排除不合法的情况，剩余的情况都可以构造。

本人选用的构造方法为：按照层序遍历顺序铺设节点，直到节点数达到要求。

## 参考程序
```cpp
void solve()
{
    ll a,b;cin >> a >> b;
    ll ub=a+b;
    if((a%2==0)||b%2){
        cout << "No" << endl;
        return ;
    }//奇偶性不符合条件
    if(b>2*a||a-1>2*b){
        cout << "No" << endl;
        return ;
    }//节点数超出临界值
    cout << "Yes" << endl;

    ll i=2,st=1,ed=1;a--;
    //[st,ed]为上一层的节点范围，i为当前节点编号
    vector<pll> son(ub+5,pll(-1,-1));
    while(a||b){
        FORLL(j,st,ed){//铺设红色节点
            if(b==0) break;
            son[j]=pll(i,i+1);
            i+=2;b-=2;
        }
        st=ed+1;ed=i-1;//更新本层的节点范围
        FORLL(j,st,ed){//铺设黑色节点
            if(a==0) break;
            if(son[j].first==-1){
                son[j]=pll(i,i+1);
                i+=2;a-=2;
            }
        }
        st=ed+1;ed=i-1;
    }
    FORLL(i,1,ub) cout << son[i].first << ' ' << son[i].second << endl;
}
```

# LM.智乃的36倍数(easy+normal)
**模数**
## 题目大意
给定$n$个$10^{18}$范围内的非负整数。
求在其中取不同的$2$个元素，拼接起来后能被$36$整除的方案数。

## 解题思路
假设$a$和$b$拼接，拼接后的数记为$c=a\times10^k+b$，其中$k$为$b$的位数。
可以发现：

$$c\\\%36=(a\times10^k+b)\\\%36 =(a\times10^k)\\\%36+b\\\%36 =(a\\\%36)\times(10^k\\\%36)+b\\\%36$$

因此，预处理$10^k\\\%36$的值，记录每个数对$36$的余数及其位数。
计算时，枚举每个数作为右半部分$b$，此时$k$也被确定为$b$的位数，
只需枚举左半部分$a$的余数，对满足条件的部分求和即可。

## 参考程序
```cpp
vector<ll> DM;
void pre(){//预处理10^k%36
    ll t=1;
    DM.pb(1);
    FORLL(i,1,19){
        t*=10;
        DM.pb(t%36);
    }
}
int dig(ll x){//求位数
    int ret=0;
    while(x){ x/=10; ret++; }
    return ret;
}
void solve()
{
    pre();
    ll n;cin >> n;
    ll t;pll tp;
    map<ll,int> mp;
    vector<pll> v;
    FORLL(i,1,n){
        cin >> t;
        tp=make_pair(t%36,dig(t));
        v.pb(tp);
        mp[t%36]++;
    }
    ll ans=0;
    for(auto &x:v){
        t=x.second;
        FORLL(i,0,35){
            if((i*DM[t]+x.first)%36==0){
                ans+=mp[i];
                if(i==x.first) ans--;
            }
        }
    }cout << ans << endl;
}
```