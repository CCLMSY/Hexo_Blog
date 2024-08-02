---
title: 题解|2024暑期杭电多校04
date: 2024/7/29
updated:
tags:
  - 2024暑期杭电多校
categories: 算法竞赛
keywords:
top_img: false
comments: true
cover:
toc: true
abbrlink: ACM_2024Summer_HDU04
description: 题解|2024暑期杭电多校04
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

比赛题单：[2024“钉耙编程”中国大学生算法设计超级联赛（4）](https://acm.hdu.edu.cn/search.php?field=problem&key=2024%A1%B0%B6%A4%B0%D2%B1%E0%B3%CC%A1%B1%D6%D0%B9%FA%B4%F3%D1%A7%C9%FA%CB%E3%B7%A8%C9%E8%BC%C6%B3%AC%BC%B6%C1%AA%C8%FC%A3%A84%A3%A9&source=1&searchmode=source)

# 1003
//TODO

# (1005)HDU7473.多层血条
## 题意
Boss血条的尺寸为 $n$ 行 $m$ 列，外层有边框。
行数仅代表血条的高度，不代表血量。
每列代表 $1$ 点血量， $m$ 列代表了 $m$ 点血量，即一层血条。

血条自底向上、从左往右平铺，最底下没有血量的部分为空格。
第一层血量用 `A` 表示，第二层血量用 `B` 表示，...，第五层血量用 `E` 表示，第六层血量用 `A` 表示，五层一循环，以此类推。

$n=2,m=10$ 且血量为$12$时，血条如下：
```
+----------+
|BBAAAAAAAA|
|BBAAAAAAAA|
+----------+
```

为了加强打击反馈，当Boss受到伤害时，即将扣减的血量将短暂显示为`.`，然后再显示为对应的血条。

如上面的血条，当受到 $3$ 点伤害时，血条将变为：
```
+----------+
|..AAAAAAA.|
|..AAAAAAA.|
+----------+
```

给定 $n,m$ ，血量 $hp$，即将受到的伤害 $dmg$ ，求受到伤害时的血条。

## 解题思路
计算血条层数决定填充当前层（和下一层）的字符。
计算当前最后一个血的位置，然后根据伤害对血条进行修改。

## 参考程序
```cpp
void solve()
{
    ll n,m,hp,dmg;
    cin >> n >> m >> hp >> dmg;
    string ul='+'+string(m,'-')+'+'; //边框
    cout << ul << endl;
    string s="|";
    ll k=(hp+m-1)/m; //层数
    char c=(k+4)%5+'A'; //当前层字符
    ll cur = (hp-1)%m+1; //当前层血量位置
    FORLL(i,1,cur) s+=c;
    char c1=(k-1)>0?(k-2)%5+'A':' '; //下一层字符
    FORLL(i,cur+1,m) s+=c1;
    s+='|';
    chmin(dmg,m);
    FORLL_rev(i,cur,1){
        if(dmg==0) break;
        s[i]='.';
        dmg--;
    }
    FORLL_rev(i,m,cur+1){
        if(dmg==0) break;
        s[i]='.';
        dmg--;
    }
    FORLL(i,1,n) cout << s << endl;
    cout << ul << endl;
}
```

# 1007
//TODO

# (1009)HDU7477.昵称检索
## 题意
给定 $n$ 个字符串表示名字。
一个昵称由两部分组成：一个给定的名字+4位数字。
其中，4位数字表示一个日期。

注：日期格式为 `MMDD` ，即月份和日期，0229也算。

给定一个长度为 $m$ 的字符串，计算所有子序列中能构成不同的昵称的个数。

## 解题思路
昵称和日期分开考虑。

要求计不同的昵称的个数，因此每种名字、日期只需要考虑一次。

名字只需要选取最早完整出现的子序列，因为早出现一定比晚出现匹配的日期个数多。
日期只需要选取最晚完整出现的子序列，因为晚出现一定比早出现匹配的名字个数多。

记录每种字符出现的位置，先匹配日期。
用 $date_i$ 表示下标为 $i$ 的位置开头的不同日期数。
对 $date$ 做一个后缀和， $date_i$ 就表示到当前位置开始，往后的不同日期数。

再匹配名字。
找到名字 $name$ 的最早出现位置 $j$ ，它能组合出的最多不同昵称数为 $date_{j+1}$，加入答案。

## 参考程序
```cpp
vector<int> days = {0,31,29,31,30,31,30,31,31,30,31,30,31};
inline int toint(char c1,char c2){
    return (c1-'0')*10+c2-'0';
}
void solve()
{
    ll n,m;cin >> n >> m;
    vector<string> names;
    string s,t;
    cin >> s;
    FORLL(i,1,n){
        cin >> t;
        names.emplace_back(t);
    }
    map<char,vector<ll>> posc,posint;
    FORLL(i,0,m-1){
        if(s[i]>='a'&&s[i]<='z') posc[s[i]].emplace_back(i);
        else posint[s[i]].emplace_back(i);
    }
    vector<ll> date(m+1,0);
    for(auto d2='0';d2<='9';d2++){
        if(posint[d2].empty()) continue; //没有这个数字
        auto posd2 = posint[d2].back(); //最后一个位置
        for(auto d1='0';d1<='3';d1++){
            auto day=toint(d1,d2);
            if(day>31||day<1) continue; //不可能的天数
            if(posint[d1].empty()) continue; //没有这个数字
            auto itposd1 = lower_bound(ALL(posint[d1]),posd2); //找比posd2小的最大的位置
            if(itposd1==posint[d1].begin()) continue; //没有比posd2小的
            auto posd1 = *(--itposd1);
            for(auto m2='0';m2<='9';m2++){
                if(posint[m2].empty()) continue; //没有这个数字
                auto itposm2 = lower_bound(ALL(posint[m2]),posd1); //找比posd1小的最大的位置
                if(itposm2==posint[m2].begin()) continue; //没有比posd1小的
                auto posm2 = *(--itposm2);
                for(auto m1='0';m1<='1';m1++){
                    auto month=toint(m1,m2);
                    if(month>12||month<1) continue;; //不可能的月份
                    if(day>days[month]) continue;; //不可能的日期
                    if(posint[m1].empty()) continue; //没有这个数字
                    auto itposm1 = lower_bound(ALL(posint[m1]),posm2); //找比posm2小的最大的位置
                    if(itposm1==posint[m1].begin()) continue; //没有比posm2小的
                    auto posm1 = *(--itposm1);
                    date[posm1]++;
                    // cout << month << ' ' << day << endl;
                }
            }
        }
    }
    FORLL_rev(i,m-1,0) date[i]+=date[i+1];
    // print_vec(date);
    ll ans=0;
    for(auto &name:names){
        ll curi=-1,len=name.length();
        FORLL(i,0,len-1){
            auto c=name[i];
            auto it=upper_bound(ALL(posc[c]),curi);
            if(it==posc[c].end()) break;
            curi=*it;
            if(i==len-1) ans+=date[curi+1];
        }
    }
    cout << ans << endl;
}
```