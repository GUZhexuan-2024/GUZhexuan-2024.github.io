---
layout: post
title: Descent Methods
subtitle: 每周一个优化算法之下降法
gh-repo: daattali/beautiful-jekyll
tags: [Optimization for Data Analysis]
comments: true
author: GU Zhexuan
---

### Preface

Happy New Year!
![happy new year](../assets/img/s-l1600.jpg)

### Main idea

In this section, we mainly consider the unconstrained optimization of a **smooth convex** (gradient can be evaluated at arbitrary point) function:

$$
\min_{x\in \mathbf{R}^{n}} f(x).
$$

Readers may refer to [Line search](../2025-09-03-Line_Search) first to have a big picture about what we're going to introduce in this blog. Now we first step into the simplest descent direction, steepest descent direction.

### Steepest-Descent Method

We have known formula of the steepest descent iteration,

$$
x_{k+1} = x_{k} - \alpha_{k}\nabla f(x_{k}),\quad k=1,2,3,...
$$

<span style="color:green"> <strong>But how to determine the step length $\alpha_{k}$?</strong> </span>

It's simple if we are dealing with the $L$-smooth function. Recall the property of $L$-smooth function,

$$
f(x+\alpha d) \le f(x) + \alpha \nabla f(x)^{\top}d + \frac{L}{2}\alpha^{2} \|d\|^{2}.
$$

Then instead of focusing on the original function, alternatively, we can minimize its upper bound which is a quadratic function of $\alpha$ given that $d = -\nabla f(x)$. Use the high school math, we can derive the minimizer is $\alpha = 1/L$. We can plug in these values and obtain an inequality,

$$
f(x_{k+1})=f(x_{k}-\frac{1}{L}\nabla f(x_{k})) \le f(x_{k})-\frac{1}{2L}\|\nabla f(x)\|^{2},
$$

which measures the amount of decrease. Depend on this fundamental inequality and other assumptions of $f$, we will derive a variety of different convergence rates in the following sections.

#### Genral Case

In this case, we assume $f$ has a global lower bound. That is, we assume there's a value $\bar{f}$ that satisfies,

$$
f(x) \ge \bar{f},\quad \mathrm{for~all~}x.
$$

Then we can sum the inequality we obtain from the last section over $k$,

$$
\begin{cases}
    f(x_{1}) \le f(x_{0}) - \frac{1}{2L}\|\nabla f(x_{0})\|^{2}, \\
    f(x_{2}) \le f(x_{1}) - \frac{1}{2L}\|\nabla f(x_{1})\|^{2}, \\
    \vdots \\
    f(x_{T}) \le f(x_{T-1}) - \frac{1}{2L}\|\nabla f(x^{T-1})\|{2}. \\
\end{cases}
$$

And we finally get,

$$
f(x_{T}) \le f(x_{0}) - \frac{1}{2L}\sum_{k=0}^{T-1}\|\nabla f(x_{k})\|^{2}.
$$

Since $\bar{f} \le f(x_{T})$, we have

$$
\sum_{k=0}^{T-1}\|\nabla f(x_{k})\|^{2} \le 2L(f(x_{0}) - \bar{f})
$$

which implies that $\lim_{T\rightarrow \infty}\|\|\nabla f(x_{T})\|\| = 0$ (<span style="color:red">If an infinite series $\sum a_{n}$ converges, then the terms $a_{n}$ must approach zero as $n$ goes to infinity</span>). Moreover, we have

$$
\min_{0\le k \le T-1}\|\nabla f(x_{k})\|^{2} \le \frac{1}{T}\sum_{k=0}^{T-1}\|\nabla f(x_{k})\|^{2}  \le \frac{2L\left[f(x_{0}) - \bar{f}\right]}{T}.
$$

Thus, we have shown that after $T$ steps of steepest descent, we can find a point $x$ satisfying

$$
\min_{0\le k \le T-1}\|\nabla f(x_{k})\| \le \sqrt{\frac{2L\left[f(x_{0}) - \bar{f}\right]}{T}}.
$$

Note that this convergence rate is slow and tells us only that we will find a point $x_{k}$ that is nearly stationary. We need to assume stronger properties of $f$ to guarantee faster convergence and global optimality.