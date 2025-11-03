<!--
 * @author: Zhexuan Gu
 * @Date: 2025-11-01 16:15:58
 * @LastEditTime: 2025-11-03 17:57:34
 * @FilePath: /GUZhexuan-2024.github.io/_posts/2025-11-01-Foundations_of_Smooth_Optimization.md
 * @Description: Please implement
-->
---
layout: post
title: Foundations of Smooth Optimization
subtitle: 每周一个优化算法之光滑优化基础知识
gh-repo: daattali/beautiful-jekyll
tags: [Optimization for Data Analysis]
comments: true
author: GU Zhexuan
---

### Preface

This new chapter is based on [Optimization for Data Analysis][1].

### Solutions to Optimization Problems

Suppose that $f$ is a function mapping some domain $\mathcal{D} = \text{dom}(f) \subset \mathbb{R}^{n}$ to the real line $\mathbb{R}$. Some key definitions are defined as follows:

1. **Local Minimizer**: $x^{\ast} \in \mathcal{D}$ is a _local minimizer_ of $f$ is there is a neighborhood $\mathcal{N}$ of $x^{\ast}$ such that $f(x) \ge f(x^{\ast})$ for all $x \in \mathcal{N} \cap \mathcal{D}$.

2. **Global Minimizer**: $x^{\ast} \in \mathcal{D}$ is a _global minimizer_ of $f$ if $f(x) \ge f(x^{\ast})$ for all $x \in \mathcal{D}$.

3. **Strict Local Minimizer**: $x^{\ast} \in \mathcal{D}$ is a _strict local minimizer_ if it is a local minimizer for some neighborhood $\mathcal{N}$ of $x^{\ast}$ and, in addition, $f(x) > f(x^{\ast})$ for all $x \in \mathcal{N}$ with $x \ne x^{\ast}$.

4. **Isolated Local Minimizer**: $x^{\ast} \in \mathcal{D}$ is a _isolated local minimizer_ if there is a neighborhood $\mathcal{N}$ of $x^{\ast}$ such that $f(x) \ge f(x^{\ast})$ for all $x \in \mathcal{N} \cap \mathcal{D}$ and, in addition, $\mathcal{N}$ contains no local minimizers other than $x^{\ast}$.

![Visualization of non-strict local minimizer and non-isoloared minimizer](../assets/img/opt_for_ds/chapter2/fig1.jpeg)

5. **Unique Minimizer**: $x^{\ast}$ is the _unique minimizer_ if it is the only global minimizer.

With the aforementioned definitions, the next challenge it to determine or judge whether a particular point is a local or global solution. Therefore, the following contents mainly focus on how to formulate the conditions to help you achieve the goal.

### Taylor's Theorem

Recall some basic knowledge first:

- Integral. 
  $$
  \int_{0}^{1}\frac{\partial f(x+\gamma p)}{\partial \gamma}d\gamma = f(x+\gamma p)\big|_{0}^{1} = f(x+p) - f(x).
  $$

- Directional Derivative. A comprehensive derivation can be found in [Line search](2025-09-03-Line_Search.md). Here, what we should know is that $\frac{\partial f(x+\gamma p)}{\partial \gamma} = \nabla f(x+\gamma p)^{\top}p$.

##### Theorem:
Given a _continuously differential_ function $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$, and given $x, p \in \mathbb{R}^{n}$, we have that

$$
\begin{array}{ll}
f(x+p) & = f(x) + \int_{0}^{1}\nabla f(x+\gamma p)^{\top}pd\gamma,    \\
f(x+p) &=  f(x) + \nabla f(x+\gamma p)^{\top}p, ~ \text{some}~\gamma \in (0,1). \\
\end{array}
$$

If $f$ is _twice continuously differentiable_, we have
$$
\begin{array}{ll}
\nabla f(x+p) & = \nabla f(x) + \int_{0}^{1}\nabla^{2} f(x+\gamma p)pd\gamma,    \\
f(x+p) &=  f(x) + \nabla f(x)^{\top}p + \frac{1}{2}p^{\top}\nabla^{2} f(x+\gamma p)p, ~ \text{some}~\gamma \in (0,1). \\
\end{array}
$$

Sometimes the second equation in the _continuously differential_ case is called the mean-value theorem.



[1]: https://icourse.club/uploads/files/bd85e2cdfb9463ca73fb2245b0f6097b3803b6e6.pdf
