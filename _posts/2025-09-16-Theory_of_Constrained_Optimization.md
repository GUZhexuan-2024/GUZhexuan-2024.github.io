---
layout: post
title: Theory of Constrained Optimization
subtitle: 每周一个优化算法之带约束优化的基础理论
gh-repo: daattali/beautiful-jekyll
tags: [Numerical Optimization]
comments: true
author: GU Zhexuan
---

### Preface

I believe I have some understanding of constrained optimization problems before exploring Chapter 12 of [Numerical Optimization][1]. For instance, during my undergraduate studies, my Calculus professor taught me the Lagrange multiplier method for tackling such problems. Additionally, through advanced machine learning and optimization courses, I learned about the penalty method and can confidently recite the Karush-Kuhn-Tucker (KKT) conditions. However, I must admit that I don’t fully grasp the deeper reasons why these methods are significant or the critical roles they play. In this blog, I will revisit these concepts, dive into their intricacies, and explore the importance of duality in constrained optimization.

### Problem formulation

In this blog, we mainly focus on the following problem,

$$
\begin{array}{ll}
    \min_{x \in \mathbb{R}^{n}} & f(x) \\
    \text{s.t.} & c_{i}(x) = 0, i \in \mathcal{E}, \\
                & c_{i}(x) \ge 0, i \in \mathcal{I},
\end{array}
$$

where $f$ and the functions $c_{i}$ are all smooth, real-valued functions on subset of $\mathbb{R}^{n}$, and $\mathcal{E}$ and $\mathcal{I}$ are finite sets of indices. Here, the $c_{i}$, $i \in \mathcal{E}$ are the equality constraints and $c_{i}$, $i \in \mathcal{I}$ are the inequality constraints. We define the feasible set $\Omega$ to be the set of points that **<span style="color:blue">satisfy the constraints</span>**.

Consequently, a simplified formulation of the problem is,

$$
\min_{x \in \Omega} ~ f(x).
$$

### Lagrangian function

I firmly believe that most readers are familiar with the structure of the Lagrangian function. Typically, we assign a multiplier to each constraint and formulate the Lagrangian by subtracting the product of each multiplier and its corresponding constraint function from the objective function, as expressed by $\mathcal{L}(x, \lambda, \mu) = f(x) - \sum_{i \in \mathcal{E}} \lambda_i c_i(x) - \sum_{i \in \mathcal{I}} \mu_i c_i(x)$. In most cases, the next step involves computing the gradient with respect to the optimization variable $x$ and setting it to zero to find the critical points. **_BUT WHY?_** 

Let's understand this with an example.

Consider

$$
\begin{array}{ll}
    \min & x_{1} + x_{2} \\
    \text{s.t.} & 2 - x_{1}^{2} - x_{2}^{2} \ge 0.
\end{array}
$$

Clearly, the feasible region is a circle and its interior as demonstrated in the following figure. Additionally, we plot the gradient of objective function $\nabla f = [1, 1]^{\top}$ and the constraint normal $\nabla c_{1} = [-2x_{1}, -2x_{2}]^{\top}$ **<span style="color:red">(The $\nabla c_{1}$ in the figure should point to the opposite direction)</span>**.

![Feasible Region](../assets/img/chapter12/fig1.png)

To analyze the problem throughly, we consider two cases:

1. **Interior Minimizer:** When the minimizer lies in the interior of the feasible set $\Omega$. In this situation, if the gradient of the objective function $\nabla f \neq \mathbf{0}$, we can move in the direction of  $-\nabla f$ with a sufficiently small step to obtain a point with a lower function value while remaining feasible. Thus, for a point in the interior to be a minimizer, it must satisfy $\nabla f = \mathbf{0}$. In this case, the zero vector is parallel to any vector, including the constraint normal.

2. **Boundary Minimizer:** When the minimizer lies on the boundary of $\Omega$. In this situation, the corresponding constraint is "active," meaning the equality $c(x) = 0$ holds. Here, not all directions are feasible, as moving in certain directions may violate the constraint. To remain feasible, a small displacement $d$ must satisfy $c(x+d)\approx c(x)+\nabla c^{\top}d = \nabla c^{\top}d\ge 0$. Additionally, to improve the objective function, we require $f(x+d) \approx f(x)+\nabla f^{\top}d < f(x) \rightarrow \nabla f^{\top}d <0$. At a minimizer, no such direction $d$ should exist that satisfies both conditions simultaneously. Consequently, $\nabla f$ and $\nabla c$ must be parallel and point in the same direction (i.e., $\nabla f = \lambda \nabla c$ for some $\lambda \geq 0$). Otherwise, the two half-spaces defined by $\nabla c^{\top} d \geq 0$ and $\nabla f^{\top} d < 0$ would intersect, implying the existence of a feasible descent direction, as illustrated in the accompanying figure.

![Feasible Region](../assets/img/chapter12/fig2.jpeg)

To sum up, both of the situations require that the gradient of the objective function is parallel to the constraint normal. Furthermore, multipler $\lambda$ is nonnegative, and when the constraint is **inactive** ($c_{i}(x) > 0$), the corresponding multiplier must be $0$ (complementarity slackness).

Back to the beginning of this section, the procudures are actually a generalization of this vivid example.

### Tagent cone and constraint qualification

All derivations appear correct at this stage. However, we should ask **whether the first-order approximation (linearization) is appropriate**.

Consider the following problem

$$
\begin{array}{ll}
    \min & x_{1} + x_{2} \\
    \text{s.t.} & 1 - x_{1}^{2} - (x_{2} - 1)^{2} \ge 0,\\
                & -x_{2} \ge 0.
\end{array}
$$

Obviously, the feasible set is the single point $\Omega = \{(0, 0)^{\top}\}$. However, if we apply linearization at the point $(0, 0)$, we are indeed looking for a direction $d$ such that $[0, -1][d_{1}, d_{2}]^{\top}\ge 0$ and $[-2x_{1}, -2(x_{2} - 1)][d_{1}, d_{2}]^{\top}\ge 0$. This implies that any direction $[d_{1}, 0]^{\top}$ maintains feasibility, which contradicts the clear conclusion that the feasible set is a single point. Thus, linearization fails to capture the geometry of the feasible set here.

To address this issue, we introduce the tangent cone, which captures the geometric features of the feasible set. We first introduce **<span style="color:red">three key definitions</span>**.

- We define the tagent cone $T_{\Omega}(x^{\ast})$ to the closed convex set $\Omega$ at a point $x^{\ast} \in \Omega$. The vector $d$ is said to be a tagent to $\Omega$ at a point $x$ is there are a feasible sequence $\{z_{k}\}$ approaching $x$ and a sequence of positive scalars $\{t_{k}\}$ with $t_{k}\rightarrow 0$ such that
$$
\lim_{k\rightarrow \infty} \frac{z_{k} - x}{t_{k}} = d.
$$
The set of all tangents to $\Omega$ at $x^{*}$ is called the tagent cone and is denoted by $T_{\Omega}(x^{\ast})$.


- We define the active set $\mathcal{A}(x)$ at any feasible $x$ consists of the equality constrain indices from $\mathcal{E}$ together with the indices of the inequality constraints $i$ for which $c_{i}(x)=0$; that is,
$$
\mathcal{A}(x) = \mathcal{E} \cup \{i\in\mathcal{I}|c_{i}(x)=0\}.
$$
At a feasible point $x$, the inequality constraint $i\in\mathcal{I}$ is said to be _active_ if $c_{i}(x)=0$ and _inactive_ if the strict inequality $c_{i}(x)>0$ is satisfied.


- Given a feasible point $x$and the active constraint set $\mathcal{A}(x)$. We define the set of linearized feasible direction $\mathcal{F}(x)$ is 
$$
\mathcal{F}(x)=
\begin{cases}
    d^{\top}\nabla c_{i}(x)=0, \text{for all } i\in \mathcal{E}, \\
    d^{\top}\nabla c_{i}(x)\ge 0, \text{for all } i\in \mathcal{A}(x) \cap \mathcal{I}.
\end{cases}
$$
It's easy to verify that $\mathcal{F}(x)$ is a cone.

The tangent cone encapsulates key geometric properties because the sequence ({z_k}) under consideration remains feasible. As a result, it encompasses directions that preserve feasibility. The textbook offers two examples to demonstrate the computation of tangents. Here I'm going to illustrate one of them.

Consider the problem,

$$
\begin{array}{ll}
    \min & x_{1} + x_{2} \\
    \text{s.t.} & x_{1}^{2} + x_{2}^{2} - 2 = 0,
\end{array}
$$

and we want to obtain the tagents at a nonoptimal point $x = (-\sqrt{2}, 0)^{\top}$. One feasible sequence we can define is, $z_{k} = (-\sqrt{2 - 1/k^{2}}, -1/k)^{\top}$, visualized as the red points in the below figure.

![Feasible Region](../assets/img/chapter12/fig3.jpeg)

Additionally, we choose $t_{k} = \|\|z_{k} - x\|\|$. The tagent computation process mainly involves the limit calculation, which is shown below.

![Feasible Region](../assets/img/chapter12/fig4.jpg)

It's satisfactory if the feasible directions $\mathcal{F}(x)$ is similar to even identicle to the tagent cone $T_{\Omega}(x)$. Constraint qualifications are pivotal in this context. The Linear Independence Constraint Qualification (LICQ) is a prominent condition, defined as:

- Given the point $x$ and the active set $\mathcal{A}(x)$, we say that the linear independence constraint qualification (LICQ) holds if the set of active constraint gradients $\{\nabla c_{i}(x), i \in \mathcal{A}(x)\}$ is linearly independent.

Consider the initial problem in this section: both constraints are active at $(0, 0)$, with gradients $(0, -1)$ and $(0, 2)$ respectively. **<span style="color:green">These gradients are evidently linearly dependent</span>**, causing the linearization at this point to inadequately reflect the geometric characteristics.

### KKT conditions

The first-order necessary consitions, also known as KKT conditions, is given in this section. 

### KKT conditions: proof

### Duality


[1]: https://www.math.uci.edu/~qnie/Publications/NumericalOptimization.pdf