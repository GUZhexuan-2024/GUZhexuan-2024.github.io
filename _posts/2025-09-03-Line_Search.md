---
layout: post
title: Line Search Algorithm
subtitle: 每日一个优化算法之线搜索
gh-repo: daattali/beautiful-jekyll
tags: [Numerical Optimization]
comments: true
mathjax: true
author: GU Zhexuan
---

### Preface

The blogs are solely used to record my study of optimization methods and, as such, will not be as comprehensive or detailed as the referenced textbook,[Numerical Optimization][1]. Additionally, as a computer science major student, I aim to keep the content practical rather than abstract.

#### Main idea

Given a continuous differentiable function $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$ and assume we are now at point $x_{k}$, since we want to minimize the function value, we aim to find a new point $x_{k+1}$, s.t., $f(x_{k+1}) < f(x_{k})$. But how to find this new point? Line search tells us to search along a direction $p_{k}$. Therefore, the iteration of line search is given by

$$
\begin{aligned}
    x_{k+1} = x_{k} + \alpha_{k}p_{k}.
\end{aligned}
$$

Another question arises, how to determine the search direction $p_{k}$. According to Taylor's Theorem,

$$
\begin{aligned}
    \label{eq:taylor}
    f(x + \alpha p) = f(x) + \alpha \nabla f(x)^{\top}p + \frac{1}{2}\left(\alpha p\right)^{\top}\nabla^{2} f(x + t\alpha p)\left(\alpha p\right), \text{for some } t \in [0, 1].
\end{aligned}
$$

Since the last term in  quadratic term about $\alpha$

[1]: https://www.math.uci.edu/~qnie/Publications/NumericalOptimization.pdf
