---
title: 题解|H.Merge the squares!-2023暑期牛客多校04
date: 2023/7/28
updated: 2023/9/28
tags:
  - 2023暑期牛客多校
  - 几何
  - 分治
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: eca5cae8
description: 题解|H.Merge the squares!-2023暑期牛客多校04 Idx:5 几何·分治
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

# H.Merge the squares!
**几何、分治**
## 题目大意
给定一个 $n\times n$ 的矩阵，它由 $n\times n$ 个小正方形组成
每次操作可以选择 $2\le x\le50$ 个正方形并把它们组合成一个更大的正方形（组合后的形状也必须为正方形）
求合并到整个边长为 $n$ 大小的正方形的操作过程
## 解题思路
这道题可以采用分治的思想

对于边长不大于 $7$ 的正方形，它的面积不大于 $49$ ，可以直接合并
对于边长较大的正方形，可以考虑将其分解为更小的正方形，递归处理

接下来考虑一种通用可行的分解方法如下图
![Img](/images/ACM/2023Summer_NCD04_H.png)
这种分法将大正方形分解成两个小正方形和两个矩形
假设小正方形在递归处理中合并完成，占用块数为 $2$ ，那么每个长方形部分允许占用的块数为 $(50-2)/2=24$ 块

对于每个长方形，按照宽分割成一个正方形和一个矩形，对两部分分别递归处理

代码中预处理了一个切割方案，用于决定在大正方形边长为 $i$ 的情况下，满足矩形 $(i-j)\times j$ 即长方形的部分，占用分割块数不超过 $24$ 时，左上角的正方形边长，以此保证每次递归处理大正方形都满足条件

# 参考代码
> 参考代码为已AC代码主干，其中部分功能需读者自行实现

```cpp
ll len[1005]={0};
void pre_len(ll n){
    FORLL(i,2,n)
        FORLL(j,1,i-1){
            ll cnt=0;
            ll a=j,b=i-j;//先减去一个j*j大小的方形
            while(b){
                cnt+=a/b;
                a%=b;
                swap(a,b);
            }//辗转相减得到(i-j)*j下分块数量
            if(cnt <= 24){
                len[i]=j;
                break;
            }//得到长i下的可用宽j
        }
}
struct node_op{
    ll x,y,n;
}top;
vector<node_op> op;
void dfs(ll x1,ll y1,ll x2,ll y2)//递归处理(x1,y1):(x2,y2)区域的矩形
{
    ll difx=x2-x1+1,dify=y2-y1+1;
    if(difx==1||dify==1) return;
    if(difx>dify){
        dfs(x1,y1,x1+dify-1,y2);
        dfs(x1+dify,y1,x2,y2);
    }else if(difx<dify){
        dfs(x1,y1,x2,y1+difx-1);
        dfs(x1,y1+difx,x2,y2);
    }else{//difx==dify
        if(difx==1) return;//边长为1无需merge
        if(difx>7){
            dfs(x1,y1,x1+len[dify]-1,y1+len[difx]-1);
            dfs(x1+len[dify],y1+len[difx],x2,y2);
            dfs(x1+len[dify],y1,x2,y1+len[difx]-1);
            dfs(x1,y1+len[difx],x1+len[dify]-1,y2);
        }
        top.x=x1;top.y=y1;top.n=difx;
        op.emplace_back(top);
    }
}
void solve()
{
    op.clear();
    ll n;
    cin >> n;
    if(n==1) {cout << "0" << endl;return;}
    pre_len(n);
    dfs(1,1,n,n);
    cout << op.size() << endl;
    for(auto i:op){
        cout << i.x << ' ' << i.y << ' ' << i.n << endl;
    }
}
```