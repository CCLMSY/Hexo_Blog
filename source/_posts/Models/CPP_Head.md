---
title: CCの算法竞赛宏定义
date: '2023/1/30 20:00'
updated: 2023/10/4 12：00
categories: 笔记模板
keywords: 宏定义
description: CCの算法竞赛宏定义
tags:
  - 模板
abbrlink: 46b4cb99
sticky:
swiper_index:
---

# CPP-Head
```cpp
#include<bits/stdc++.h>
using namespace std;

/*----------Configurations Area----------*/
const long MOD=1e9+7;
#define N 100005
/*----------Configurations Area----------*/

const double pi=3.141592653589793;
const double eps=1e-8;
const double goden_radio=1.618033988749895;
const long long INF=0x3fffffffffffffff;

typedef long long ll;
typedef pair<ll,ll> pll;

namespace DEFINITION
{
    #define scanfll(a) scanf("%lld",&a)
    #define lowbit(x) ((x)&(-(x)))
    #define RESET(A) memset(A,0,sizeof(A))
    #define ALL(A) A.begin(),A.end()
    #define SORT(A) sort(ALL(A))
    #define Presentation(i,r) " \n"[i==r]
    #define FORLL(i,l,r) for(ll i=l;i<=r;i++)
    #define Get_Mod(a) (((a)+MOD)%MOD)
    #define NO "NO\n"
    #define YES "YES\n"
}

namespace CCLIB
{
    #define print_vec(A) for(auto i:A) cout << i << ' ';cout << endl;
    #define print_float(value,digit) cout << fixed << setprecision(digit) << value;

    //扩欧返回d=gcd(a,b);x,y对应ax+by=d的解
    ll Exgcd(ll a,ll b,ll &x,ll &y) {if(a==0&&b==0) return -1; if(b==0) {x=1;y=0;return a;} ll d=Exgcd(b,a%b,y,x); y-=a/b*x; return d;}
    //扩欧求解a在n下的乘法逆元
    ll Get_Inv(ll a,ll n) {ll x,y; ll d=Exgcd(a,n,x,y); if(d==1) return (x%n+n)%n; else return -1;}
    ll qcpow(ll x, ll b) {
        ll ret = 1;x=Get_Mod(x);
        for(; b; b >>= 1, x = 1ll * x * x % MOD)
            if(b & 1) ret = 1ll * ret * x % MOD;
         return ret;}

    vector<ll> Fac,Fac_inv;
    void Prepare_Factorium(ll n){
        Fac.clear();Fac.resize(n+1);Fac[0]=Fac[1]=1;
        Fac_inv.clear();Fac_inv.resize(n+1);Fac_inv[0]=Fac_inv[1]=1;
        FORLL(i,2,n) {Fac[i]=Get_Mod(Fac[i-1]*i);Fac_inv[i]=Get_Inv(Fac[i],MOD);}
    }void Prepare_Combination(ll n){Prepare_Factorium(n);}
    ll Get_Combination(ll m,ll n){
        return Get_Mod(Get_Mod(Fac[m]*Fac_inv[m-n])*Fac_inv[n]);
    }
}

namespace MOLDULE
{
    ll add(ll x, ll y) {return Get_Mod(x + y);}
    ll addto(ll &x, ll y) {return x = add(x, y);}
    ll sub(ll x, ll y) {return Get_Mod(x - y);}
    ll subto(ll &x, ll y) {return x = sub(x, y);}
    ll mul(ll x, ll y) {return 1ll * x * y % MOD;}
    ll multo(ll &x, ll y) {return x = mul(x, y);}
    ll inv(ll x) {return CCLIB::Get_Inv(x,MOD);}
}


#define ONLINE_JUDGE
#define IO_OPTIMIZE
//#define MUTIPLE_JUDGE
//#define CHECK_OUT_TIME

using namespace DEFINITION;
//using namespace MOLDULE;
using namespace CCLIB;

/*----------Code Area----------*/
void solve()
{

}
/*----------Code Area----------*/

int main(){
    int clk = clock();

#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif

#ifdef IO_OPTIMIZE
    ios::sync_with_stdio(false);
    cin.tie(nullptr); cout.tie(nullptr);
#endif

#ifdef MUTIPLE_JUDGE
    long T; cin >> T;
    while(T--) solve();
#else
    solve();
#endif

#ifdef CHECK_OUT_TIME
    cout << clock() - clk << " ms\n";
#endif
    return 0;
}
```