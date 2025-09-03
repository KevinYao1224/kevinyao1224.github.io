---
layout: page
title: 简述一类 Subset Sum 问题
permalink: archive/2025/09/03/简述一类-Subset-Sum-问题
categories: XCPC
excerpt: 对于物品大小最大为 $V$ 的 Subset Sum 问题可以做到 $\mathrm{O}(nV)$ 的时间复杂度, 而传统背包 DP 解法只能做到 $\mathrm{O}(n^2V)$.
---

> 这是一个非常经典的问题, 例题可以见 QOJ-7403, 标题就是 Subset Sum.

考虑这样的问题: 给出 $n$ 个物品, 第 $i$ 个物品的大小是 $a_i$, 且 $1 \leq a_i \leq V$, 给出一个容量 $C$, 求选择这些物品的一个子集, 使得总大小不超过 $C$ 的情况下, 总大小最大是多少.

不难注意到, 有意义的 $C < \sum a_i \le nV$. 否则可以选择全部的物品. 所以采用一般的背包 DP 解法, 可以在 $\Theta(nC) = \mathrm{O}(n^2V)$ 的时间内解决这个问题. 现在给出一种 $\mathrm{O}(nV)$ 的解法.

首先, 用贪心的方式, 扫描所有物品, 能加入就加入. 设 $c$ 表示剩余的背包容量. 显然, 对于有意义的 $C$, 最终 $0 < c < V$, 有一些物品已经被加入背包, 有一些物品目前无法加入背包. 前者记为集合 $A$, 后者记为集合 $B$. 我们把当前的解称为初始解, 尝试从初始解调整得到一种最优解.

不难证明, 任何一种合法的最优解一定可以用这样的方式从初始解调整而来: 我们允许在中间过程中将背包加到"溢出", 即使得 $c < 0$. 每次若当前 $c = 0$, 则已经获得一个最优解, 直接输出答案 $C$; 若当前 $c > 0$, 则必须从 $B$ 中选择一个未加入的物品, 将它加入(但是不将其转移到 $A$ 中); 若当前 $c < 0$, 则必须从 $A$ 中选择一个已加入的物品, 将它反悔(但是不将其转移到 $B$ 中).

下面考虑这样的 DP, 设 $p_{k, i, j}$ 表示如果只考虑 $A$ 中前 $i$ 个元素和 $B$ 中前 $j$ 个元素, 那么可不可以令 $c = k$. 显然, 这个数组具有关于 $i$ 和 $j$ 的单调性, 所以可以改进为这样的记法: 设 $f_{i, k}$ 表示只考虑 $A$ 中前 $i$ 个元素, 要使得 $c = k$, 至少需要考虑 $B$ 中前几个元素(如果不存在这样的 $j$ 则令 $f_{i, k} = \vert B \vert + 1$). 显然, 这样状态集合的大小是 $\Theta(nV)$ 的.

转移有以下 3 种:

1. $f_{i, k} \to f_{i + 1, k}$.
1. 若 $k < 0$, 则 $f_{i, k} \to f_{i + 1, k + A_{i + 1}}$.
1. 若 $k > 0, f_{i, k} < j$, 则 $j \to f_{i + 1, k - B_{j}}$.

注意到只有第 3 种转移不是 $\Theta(1)$ 的, 考虑针对它进行优化.

由于第 1 种转移的存在, 因此 $f$ 关于 $i$ 单调减. 如果 $k > 0, i \geq 1, j \ge f_{i - 1, k}$, 那么必然存在转移 $j \to f_{i - 1, k - B_j}$, 又存在 $f_{i - 1, k - B_j} \to f_{i, k - B_j}$, 所以对于每个 $f_{i, k}$, 只需要尝试 $f_{i, k} \le j < f_{i - 1, k}$ 的 $j$ 来进行第 3 类转移. 根据 $f$ 关于 $i$ 的单调性, 这时第 3 种转移的总复杂度是 $\mathrm{O}(nV)$ 的.

QOJ 7403 的参考代码如下:

```cpp
#include <bits/stdc++.h>

using namespace std;

using LL = long long;
using ULL = unsigned long long;

#define F(i, l, r) for (int i = (l); i <= (r); ++i)
#define FF(i, r, l) for (int i = (r); i >= (l); --i)
#define MAX(a, b) ((a) = max((a), (b)))
#define MIN(a, b) ((a) = min((a), (b)))
#define SZ(a) ((int)(a).size() - 1)

constexpr int N = 20005, V = 20000;

int n1, n2, c, w0;
int a[N], b[N];
int f[2][V * 2 + 10];

void work(void)
{
    {
        int n;
        cin >> n >> c;
        vector<int> vec(n);
        F(i, 0, n - 1) cin >> vec[i];
        sort(vec.begin(), vec.end());
        int cur = 0;
        n1 = 0, n2 = 0;
        FF(i, n - 1, 0)
        {
            if (cur + vec[i] <= c) cur += vec[i], b[++n2] = vec[i];
            else a[++n1] = vec[i];
        }
        if (n1 == 0 || n2 == 0 || cur == c)
        {
            cout << cur << '\n';
            return;
        }
        w0 = c - cur;
    }

    int ll = -a[1], rr = max(b[1], w0);
    F(i, ll, rr)
    {
        f[0][i + V] = n2 + 1;
    }
    f[0][w0 + V] = 0;
    F(i, 1, n1)
    {
        F(x, ll, rr)
        {
            f[i & 1][x + V] = f[i & 1 ^ 1][x + V];
            if (x + a[i] >= 0 && x + a[i] <= rr) MIN(f[i & 1][x + V], f[i & 1 ^ 1][x + a[i] + V]);
        }
        F(x, ll, 0)
        {
            F(j, f[i & 1][x + V] + 1, min(f[i & 1 ^ 1][x + V], n2))
            {
                MIN(f[i & 1][x + b[j] + V], j);
            }
        }
        if (f[i & 1][V] <= n2)
        {
            cout << c << '\n';
            return;
        }
    }
    F(i, 0, rr) if (f[n1 & 1][i + V] <= n2)
    {
        cout << c - i << '\n';
        return;
    }
    assert(0);
}

signed main(void)
{
    ios::sync_with_stdio(false), cin.tie(nullptr);

    int _;
    cin >> _;
    while (_--) work();

    return 0;
}

```
