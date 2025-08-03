---
layout: page
title: HDU6190 Law of Commutation
permalink: archive/2025/07/30/HDU6190-Law-of-Commutation
categories: XCPC
excerpt: 解数论方程 $a^b \equiv b^a \pmod{2^n}$
---

> 参考:
> 
> - <https://blog.csdn.net/V5ZSQ/article/details/79325038>
> - <https://www.codeleading.com/article/49961848995>

不难想到分奇偶讨论.

如果 $a$ 是奇数. 那么 $b$ 也应该是奇数.

因为 $a$ 是奇数, 所以 $a^2 \equiv 1 \pmod {2^3}$, 所以 $a^b \equiv a \pmod {2^3}$. 同理, $b^a \equiv b \pmod {2^3}$, 所以 $a \equiv b \pmod {2^3}$.

因为 $a^b \equiv a^{b \bmod 2^3} \pmod {2^4}$, 且同理 $b^a \equiv b^{a \bmod 2^3} \equiv b^{b \bmod 2^3} \pmod {2^4}$, 所以 $(ab^{-1})^{b \bmod 2^3} \equiv 1 \pmod {2^4}$. 假设 $ab^{-1} \not\equiv 1 \pmod {2^4}$, 那么 $\vert ab^{-1}\vert \vert 2^3$, 所以它是一个偶数, 而 $b \bmod 2^3$ 是一个奇数, 所以不可能使得 $\vert ab^{-1}\vert\vert b \bmod 2^3$, 所以矛盾. 所以, $ab^{-1} \equiv 1 \pmod {2^4}$, 所以 $a \equiv b \pmod {2^4}$.

重复上面的过程, 用数学归纳法, 可以证明 $a \equiv b \pmod {2^n}$. 所以, 当 $a$ 为奇数时, 答案是 $1$.

如果 $a$ 是偶数, 那么 $b$ 也是偶数. 如果 $1 \le b \le n$, 那么 $b$ 只有很少的可能性, 暴力枚举即可.

如果 $n < b \le 2^n$, 那么 $a^b \equiv 0 \pmod {2^n}$. 所以我们只关心在 $(n, 2^n]$ 中有多少整数 $b$ 满足 $b^a\equiv 0 \pmod {2^n}$. 显然, 假设 $x$ 的质因子 $2$ 的指数是 $c_2(x)$, 那么这一限制就是说 $c_2(b) \ge \left\lceil\frac{n}{a}\right\rceil$, 也就是说 $\left.2^{\left\lceil\frac{n}{a}\right\rceil}\right\vert b$. 这是非常好求的.

```cpp
// Author: kyEEcccccc

#include <bits/stdc++.h>

using namespace std;

using LL = long long;
using ULL = unsigned long long;

#define F(i, l, r) for (int i = (l); i <= (r); ++i)
#define FF(i, r, l) for (int i = (r); i >= (l); --i)
#define MAX(a, b) ((a) = max(a, b))
#define MIN(a, b) ((a) = min(a, b))
#define SZ(a) ((int)((a).size()) - 1)

int kpow_mod(int x, int k, int n)
{
    int m = (1 << n) - 1;
    x &= m;
    int r = 1;
    while (k)
    {
        if (k & 1) r = r * x & m;
        x = x * x & m;
        k >>= 1;
    }
    return r;
}

signed main(void)
{
    // freopen(".in", "r", stdin);
    // freopen(".out", "w", stdout);
    ios::sync_with_stdio(0), cin.tie(nullptr);

    int n, a;
    while (cin >> n >> a)
    {
        if (a & 1) cout << 1 << '\n';
        else
        {
            int tot = 0;
            F(b, 1, n)
            {
                if (kpow_mod(a, b, n) == kpow_mod(b, a, n))
                {
                    ++tot;
                }
            }
            int x = (n - 1) / a + 1;
            cout << tot + ((1 << n) >> x) - (n >> x) << '\n';
        }
    }
    
    return 0;
}
```