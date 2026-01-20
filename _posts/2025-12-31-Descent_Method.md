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


