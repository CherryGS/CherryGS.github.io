---
layout: post
title: cf1882e1 - Two Permutations (Easy Version)
date: '2023-09-26 22:26:23'
permalink: /post/cf1882e1-two-permutations-easy-version-1o6dot.html
tags:
  - Inverse Pairs
categories:
  - competitive programming
  - codeforces
published: true
---





# [cf1882e1 - Two Permutations (Easy Version)](https://codeforces.com/contest/1882/problem/E1)

## 描述

## 思路

> * ​#逆序对#​(的奇偶性)是序列的一个非常重要的条件 , 部分情况下可以作为必要条件来证伪
> * 尝试将对区间进行的(交换)操作合并为对两个位置进行的(交换)操作

最重要的两个性质

1. 如果序列长度 N 为奇数 , 那么在位置 1 处操作 N 次可以得到原序列. 这条性质说明 N 为奇数时 , 操作数的奇偶性可以调整.
2. 如果序列长度 N 为偶数 , 那么每一次操作都会导致逆序对数量的奇偶性发生变化. 这条性质说明 N 为偶数时 , 操作数的奇偶性无法调整.

首先我们通过一种操作方法得到了 p 和 q 还原的操作数 x 和 y

**如果 x 和 y 奇偶性相等**

此时对于操作数较小的序列只需要循环选定一个数(不是位置)操作直到两序列操作数相等

**如果 x 和 y 奇偶性不等但是序列长度 n 和 m 其中有一个为奇数**​

此时运用性质 1 , 可以使得奇偶性相等

**如果 x 和 y 奇偶性不等但是序列长度 n 和 m 都为偶数**

此时运用性质 2 , 无解

---

性质 1 证明: 在位置 1 操作就相当于将第一个数扔到末尾 , 扔一轮就是原序列

性质 2 证明:

首先 , 易证交换相邻两个数可以改变逆序对数的奇偶性

那么假设序列长度 N 为偶数 , 一个操作将序列分为了 x 1 y 三部分 , 操作后为 y 1 x

我们将操作看作若干次相邻交换的合成

那么上述操作进行的相邻交换数即为 $y + x * (y+1)$ , 其中 $x+y = N-1$ , $N-1$ 为奇数

不难发现上式一定为奇数 , 这就证明了性质 2

---

这里说一下我的操作方法

我的操作方法目的是将数 x 移动到 x+1~n 前面

首先将 x+1~n 移动到开头(通过选择数 x+1 的前一个位置)

再将数 x 移动到 x+1 前面(通过选择数 x 的位置)

大概要 2N 次

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

std::mt19937 rng(std::random_device{}());
typedef long double ld;
typedef long long ll;
typedef unsigned long long ull;
typedef const int& cint;
typedef const ll& cll;
typedef pair<int, int> pii;
typedef pair<int, ll> pil;

#define ls (loc<<1)
#define rs ((loc<<1)|1)

const int mod1 = 1e9+7;
const int mod2 = 998244353;
const int inf_int = 0x7fffffff;
const int hf_int = 0x3f3f3f3f;
const ll inf_ll = 0x7fffffffffffffff;
const double ept = 1e-9;

int n, m;
int a[3000], b[3000];
int t[3003];
vector<int> q[2];

void sw(int x, int l, int p[]) {
    assert(x > 0);
    int c = 0;
    for(int i=x+1; i<=l; i++) { t[++c] = p[i]; }
    t[++c] = p[x];
    for(int i=1; i<x; i++) { t[++c] = p[i]; }
    for(int i=1; i<=l; i++) { p[i] = t[i]; }
}

int fd(int x, int l, int p[]) {
    for(int i=1; i<=l; i++) {
        if(p[i] == x) { return i; }
    }
    return -1;
}

void solve(cint T) {
    cin >> n >> m;
    q[0].clear(); q[1].clear();
    for(int i=1; i<=n; i++) { cin >> a[i]; }
    for(int j=1; j<=m; j++) { cin >> b[j]; }
    for(int i=n; i>=2; i--) {
        int i1, i2;
        i1 = fd(i, n, a);
        if(i1 != 1) {
            sw(i1-1, n, a);
            q[0].push_back(i1-1);
        }
        i2 = fd(i-1, n, a);
        sw(i2, n, a);
        q[0].push_back(i2);
    }
    for(int i=m; i>=2; i--) {
        int i1, i2;
        i1 = fd(i, m, b);
        if(i1 != 1) {
            sw(i1-1, m, b);
            q[1].push_back(i1-1);
        }
        i2 = fd(i-1, m, b);
        sw(i2, m, b);
        q[1].push_back(i2);
    }
    int q0 = q[0].size(), q1 = q[1].size();

    // for(int i=1; i<=n; i++) { cout << a[i] << ' ';  }
    // cout << endl;
    // for(int i=1; i<=m; i++) { cout << b[i] << ' ';  }
    // cout << endl;
    // cout << q0 << ' ' << q1 << endl;

    if(abs(q0-q1) & 1) {
        if(n % 2 <span style="font-weight: bold;" class="mark"> 0 && m % 2 </span> 0) {
            cout << -1 << '\n';
            return;
        }
        else if(n % 2 == 0) {
            for(int i=1; i<=m; i++) { q[1].push_back(1); }
        }
        else {
            for(int i=1; i<=n; i++) { q[0].push_back(1); }
        }
    }
    q0 = q[0].size(), q1 = q[1].size();
    cout << max(q0, q1) << '\n';
    for(int i=0; i<max(q0, q1); i++) {
        if(i < q0) { cout << q[0][i] << ' '; }
        else { cout << ((i&1)?1:n) << ' '; }
        if(i < q1) { cout << q[1][i] << ' '; }
        else { cout << ((i&1)?1:m) << ' '; }
        cout << '\n';
    }
}

int main() {
    //freopen("1.in", "r", stdin);
    //cout.flags(ios::fixed); cout.precision(8);
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    int T_=1;
    //std::cin >> T_;
    for(int _T=1; _T<=T_; _T++)
        solve(_T);
    return 0;
}
```
