---
title: 题解|2023暑期杭电多校09
date: 2023/8/15
updated: 2023/11/9
tags:
  - 2023暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2023Summer_HDU09
description: 题解|2023暑期杭电多校09
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

# 1002.Shortest path
**~~签到题~~** **DFS记忆化搜索**
## 题目大意
给定一个正整数 $n$ ，可以对其进行以下操作：
1. 如果 $n$ 能被 $3$ 整除，则可以使 $n=n/3$ ;
2. 如果 $n$ 能被 $2$ 整除，则可以使 $n=n/2$ ;
3. 使 $n=n-1$ 
   
求使得 $n$ 变成 $1$ 的最少操作次数

## 解题思路
~~将样例Output输出即可~~
> 这题不难，但确实精彩()//毕竟……
> 《钉耙编程”中国大学生算法设计超级联赛》是由hdu自主研发的一款全新开放世界冒险竞赛。竞赛发生在一个被称作“hdu”的幻想世界，在这里，被编译器选中的人将被授予“C++”，导引代码之力。你将扮演一位名为“acmer”的神秘角色，在自由的打题中邂逅性格各异能力独特的STL容器，和他们一起击败强题，找回AC的代码

不闹了，解题吧

不难看出操作 $3$ 的收益最低，是不满足操作 $1,2$ 的时候凑条件用的。
而由于只允许整除，操作 $1,2$ 的优劣性不好评估（因为要夹杂操作 $3$ 而不单纯是减少的量的区别），因此每次对本次进行的两种操作方案进行比较。

按以下操作递归处理 $n$ ：
1. 如果 $n=1$ ，则返回 $0$ ；
2. 进行若干次（可能0次）操作 $3$ 使得 $n$ 能被 $2$ 整除，执行操作 $2$
3. 进行若干次（可能0次）操作 $3$ 使得 $n$ 能被 $3$ 整除，执行操作 $1$

由于数据范围的关系，传统的DFS会超时，因此需要使用记忆化搜索
即每次计算完某个数（记为 $x$ ）的结果，将其保存下来，后续搜索 $x$ 时就无需继续搜索到底部，直接输出这个数的结果即可
记忆化搜索可以用 map 实现，频繁读取而不考虑元素顺序的可以使用 unordered_map ，有效降低时间空间复杂度

![Img](/images/ACM/2023Summer_HDU09_1002.png)

第2、3行的提交使用了 map ，代码完全一致；第1行的提交仅仅将 map 改为了 unordered_map 

### 时间复杂度
$O(t\log^2n)$

## 参考代码
```cpp
unordered_map<ll,ll> mp;
ll dfs(ll n){
    if(n<=1) return 1-n;
    if(mp[n]) return mp[n];
    ll t1,t2;
    t1=n%2+1+dfs(n/2);
    t2=n%3+1+dfs(n/3);
    return mp[n]=min(t1,t2);
}
void solve()
{
    ll n;cin >> n;
    cout << dfs(n) << endl;
}
```

***

# 1005.List Reshape
**签到题、模拟**
## 题目大意
按一定格式给定一个纯数字一维数组，按给定格式输出成二维数组。

## 解题思路
读入初始数组字符串，将每个数字分离，按要求输出即可

## 参考代码
```cpp
#define N 100005
char s[N]={0};
vector<string> nums;
void getnums(){
    string ts;ll len=strlen(s);char c;
    FORLL(i,0,len){
        c=s[i];
        if(c>='0'&&c<='9') ts.push_back(c);
        else if(ts.size()){
            nums.emplace_back(ts);
            ts.clear();
        }
    }
}
void solve()
{
    getchar();gets(s);
    ll n,m;scanf("%lld%lld",&n,&m);
    nums.clear();getnums();
    //print_vec(nums);
    cout << '[';
    FORLL(i,0,n-1){
        cout << '[';
        FORLL(j,0,m-1){
            cout << nums[i*m+j];
            if(m-1-j) cout << ", ";
        }cout << "]";
        if(n-1-i) cout << ", ";
    }cout << ']' << endl;
}
```

***

# 1008.Coins
**概率论**
## 题目大意
$n$ 个人进行游戏，每个人初始有 $a_i$ 个硬币，每次随机选择一个人给另一个人一枚硬币
若某人给出硬币后没有剩余，则退出游戏。直到有人拿到所有硬币游戏结束
求游戏轮数的数学期望

## 解题思路
概率论还没学（）官方题解的鞅论看不懂（躺）//
等学完回来给严格推导（手搓大饼ing）
ACMer三大美德：暴力、打表、猜结论//下面给出道德解法（）

假设现在只有两个人，那么每个人给对方硬币的概率都为50%，直到其中一人没有硬币为止。
设这两个人的硬币数分别为 $m,n$ ，则游戏轮数期望为：
$f(m,n)=1+\dfrac{1}{2}f(m+1,n-1)+\dfrac{1}{2}f(m-1,n+1)$

可以发现越往后对期望的贡献越低，限制递归深度，利用如下程序暴力打表：
```cpp
double f(ll m,ll n,ll rnd){
    if(rnd>40) return 0;//递归上限40层（已经要跑很久了）
    if(m==0||n==0) return 0;
    return 1+0.5*f(m+1,n-1,rnd+1)+0.5*f(m-1,n+1,rnd+1);
}
void solve()
{
    ll n,m;cin >> n >> m;
    cout << f(m,n,0) << endl;
}
```
得到以下打表结果（不得不说跑的是真慢）：
```
in:1 3 out:3
in:1 5 out:4.98732
in:2 3 out:5.99897
in:2 5 out:9.8626
in:3 4 out:11.8287
```

暴力打完表可以猜结论了： $f(m,n)=mn$ ，即对于两个人，答案是他们硬币数的乘积；
那么对于三个人，利用样例`in:1 1 1 out:3`盲猜是 $a_1a_2+a_1a_3+a_2a_3$ 
推广到 $n$ 个人，两两相乘再相加即可

由于数据较大，答案需要开int128才能放得下
另外结合数据范围，利用前缀和算出结果即可

### 时间复杂度
前缀和&求和：$O(n)$

## 参考代码
```cpp
void solve()
{
    int128 res;
    ll n,t;cin >> n;
    vector<ll> v(n+1),sum(n+1,0);
    FORLL(i,1,n){
        cin >> v[i];
        sum[i]=v[i]+sum[i-1];
    }
    FORLL_rev(i,n,1){
        res=res+sum[i-1]*v[i];
    }
    res.print();printf("\n");
    return;
}
```