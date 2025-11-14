---
layout: page
title: 无穷集都有可数无穷子集在 ZFC 中的证明
permalink: archive/xxxx/xx/xx/无穷集都有可数无穷子集在-ZFC-中的证明
categories: 数学
excerpt:
---

## 1. 自然数集合的存在性

为了可读性, 本文会省略空集公理, 外延公理, 配对公理与并集公理的运用. 具体地, 当我们写 $x = \varnothing$, 这就是说 $\forall y(\lnot y \in x)$; 当我们写 $z = x \cup y$, 这就是说 $\forall t((t \in z \to t \in x \lor t \in y) \land (t \in z \gets t \in x \lor t \in y))$; 当我们写 $x = \{a_0, a_1\}$, 这就是说 $\forall y((y \in x \to y = a_0 \lor y = a_1) \land (y \in x \gets y = a_0 \lor y = a_1))$.

无穷公理: $\exists S_0(\varnothing \in S_0 \land \forall n(n \in S_0 \to n \cup \{n\} \in S_0))$. 这个集合包含标准自然数, 但是也可以包含一些其他元素, 下面我们从中分离出一个遵从数学归纳法原理的自然数集合.

考虑谓词 $Num(n) := (n = \varnothing \lor \exists m \in n(n = m \cup \{m\})) \land \forall t \in n(t = \varnothing \lor \exists m \in n \cap t(t = m \cup \{m\}))$. 应用分离公理模式, 从无穷公理给定的 $S_0$ 中分离出集合 $\mathbb{N} = \{x \in S_0 : Num(x)\}$. 在这个集合中, 指定 $0 := \varnothing$, 以及后继函数 $Next(x) = x \cup \{x\}$.

下面我们证明, 这个集合与指定的两种符号兼容皮亚诺公理体系:

1. $\forall x \in \mathbb{N}(Next(x) \in \mathbb{N})$.
1. $\forall x, y \in \mathbb{N}(x \neq y \to Next(x) \neq Next(y))$.
1. $\forall x \in \mathbb{N}(Next(x) \neq 0)$.
1. 数学归纳法公理模式: $[P(0) \land \forall n \in \mathbb{N}(P(n) \to P(Next(n)))] \to \forall n \in \mathbb{N}(P(n))$.