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

Happy New Year! This new chapter is based on [Optimization for Data Analysis][1].
![happy new year](../assets/img/s-l1600.jpg)

### Main idea

In this section, we mainly consider the unconstrained optimization of a **smooth convex** (gradient can be evaluated at arbitrary point) function:

$$
\min_{x\in \mathbb{R}^{n}} f(x).
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
f(x_{k+1})=f(x_{k}-\frac{1}{L}\nabla f(x_{k})) \le f(x_{k})-\frac{1}{2L}\|\nabla f(x_{k})\|^{2},
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
    f(x_{T}) \le f(x_{T-1}) - \frac{1}{2L}\|\nabla f(x_{T-1})\|{2}. \\
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

#### Convex Case

##### Theorem 1

Suppose that $f$ is convex and $L$-smooth, and suppose that the problem $\min_{x\in \mathbb{R}^{n}} f(x)$ has a solution $x^{\ast}$. Define $f^{\ast}:= f(x^{\ast})$. Then the steepest-descent method with steplength $\alpha_{k} \equiv \frac{1}{L}$ generates a sequence $\{x_{k}\}_{k=0}^{\infty}$ that satisfies,

$$
f(x_{T}) - f^{\ast} \le \frac{L}{2T}\|x_{0} - x^{\ast}\|^{2},\quad T = 1,2,3...
$$

##### Proof 1

According to the property of the convex function, we have $f(x^{\ast}) \ge f(x) + \nabla f(x)^{\top}(x-x^{\ast})$. Thus, we have,

$$
\begin{array}{ll}
f(x_{k+1}) &\le f(x_{k}) - \frac{1}{2L}\|\nabla f(x_{k})\|^{2} \\
           &\le f(x^{\ast}) - \nabla f(x_{k})^{\top}(x_{k} - x^{\ast}) - \frac{1}{2L}\|\nabla f(x_{k})\|^{2} \\
           &= f(x^{\ast}) + \frac{L}{2}\left( \|x_{k}-x^{\ast}\|^{2} -\|x_{k}-x^{\ast} - \frac{1}{L}\nabla f(x_{k})\|^{2} \right) \\
           &= f(x^{\ast}) + \frac{L}{2}\left( \|x_{k}-x^{\ast}\|^{2}- \|x_{k+1}-x^{\ast}\|^{2} \right).
\end{array}
$$

Then similarly, we can sum over $k$,

$$
\begin{cases}
    f(x_{1}) \le f(x^{\ast}) + \frac{L}{2}\left( \|x_{0}-x^{\ast}\|^{2}- \|x_{1}-x^{\ast}\|^{2} \right), \\
    f(x_{2}) \le f(x^{\ast}) + \frac{L}{2}\left( \|x_{1}-x^{\ast}\|^{2}- \|x_{2}-x^{\ast}\|^{2} \right), \\
    \vdots \\
    f(x_{T}) \le f(x^{\ast}) + \frac{L}{2}\left( \|x_{T-1}-x^{\ast}\|^{2}- \|x_{T}-x^{\ast}\|^{2} \right), \\
\end{cases}
$$

and obtain,

$$
\sum_{k=0}^{T-1}(f(x_{k})-f(x^{\ast})) \le \frac{L}{2}\left(\|x_{0}-x^{\ast}\|^{2} - \|x_{T}-x^{\ast}\|^{2}\right) \le \frac{L}{2}\|x_{0}-x^{\ast}\|^{2}.
$$

Since $\{f(x_{k})\}$ is a nonincreasing sequence, we have

$$
f(x_{T}) - f(x^{\ast}) \le \frac{1}{T}\sum_{k=0}^{T-1}(f(x_{k})-f(x^{\ast})) \le \frac{L}{2T}\|x_{0}-x^{\ast}\|^{2}.
$$

<span style="color:blue">Proof complete.</span>

#### Strongly Convex Case

Before diving into the details of the convergence analysis, we first talk about a simple fact. 

**Any convex function can be perturbed to form a stronly convex function by adding any small positive multiple of the squared Euclidean norm. In fact, if $f$ is any $L$-smooth function, then**

$$
f_{\mu}(x) = f(x) + \mu \|x\|^{2}
$$

**is stronly convex for $\mu$ large enough.**

##### Proof 2

The convex case is straightforward to prove. To prove $f_{\mu}(x)$ is strongly convex, we need to establish:

$$
\begin{array}{ll}
    f_{\mu}(y) &\ge f_{\mu}(x) + \nabla f_{\mu}(x)^{\top}(y-x) + \frac{m}{2}\|y-x\|^{2}, \\
    f(y) + \mu \|y\|^{2} &\ge f(x) + \mu \|x\|^{2} + \langle \nabla f(x) + 2\mu x, y-x\rangle + \frac{m}{2}\|y-x\|^{2}, \\
    f(y) &\ge f(x) +  \langle \nabla f(x), y-x\rangle + (\frac{m}{2}-\mu)\|y-x\|^{2}.
\end{array}
$$

By convexity of $f$,

$$
f(y) \ge f(x) +  \langle \nabla f(x), y-x\rangle
$$

for all $x, y$. Thus, for any $\mu > 0$, setting $m=2\mu$ satisfies the inequality and $f_{\mu}(x)$ is therefore $2\mu$-strongly convex.

For a general $L$-smooth function $f$, we use definition of $L$-smoothness and Cauchy-Schwarz inequality to derive,

$$
\langle \nabla f(y) - \nabla f(x),y - x \rangle \ge -\|\nabla f(y) - \nabla f(x)\| \|y - x\| \ge -L \|y - x\|^{2}.
$$

Now compute the new term,

$$
\begin{array}{ll}
\langle \nabla f_{\mu}(y) - \nabla f_{\mu}(x),y - x \rangle &= \langle \nabla f(y) - \nabla f(x),y - x \rangle  + 2\mu \|y - x\|^{2} \\
& \ge (2\mu - L) \|y - x\|^{2}.
\end{array}
$$

Recall strong convexity is equavalent to the condition,

$$
\begin{array}{ll}
    f_{\mu}(y) &\ge f_{\mu}(x) + \nabla f_{\mu}(x)^{\top}(y-x) + \frac{m}{2}\|y-x\|^{2},  \\
    f_{\mu}(x) &\ge f_{\mu}(y) + \nabla f_{\mu}(y)^{\top}(x-y) + \frac{m}{2}\|x-y\|^{2},  \\
    \langle \nabla f_{\mu}(y) - \nabla f_{\mu}(x),y - x \rangle &\ge m \|y - x\|^{2}.
\end{array}
$$

Therefore, when $\mu > \frac{L}{2}$, we can choose $m = 2\mu-L > 0$, which guarentees that $f_{\mu}(x)$ is $m$-strongly convex.

<span style="color:blue">Proof complete</span>.

We are in favor of strongly convex functions because of its nice properties. First, the norm of the gradient can provide information about far away we are from optimality. Given the property of the stronlgy convex function $f$,

$$
f(z) \ge f(x) + \nabla f(x)^{\top}(z - x) + \frac{m}{2}\|z-x\|^{2}
$$

and suppose we are minimizing both sides over $z$. Obviously, the minimizer on the left-hand side is attained at $z=x^{\ast}$, while on the right-hand side, it's attained at $z=x - \frac{1}{m}\nabla f(x)$. Plug in the minimizers, we obtain

$$
\begin{array}{ll}
    f(x^{\ast}) &\ge f(x) - \nabla f(x)^{\top}(\frac{1}{m}\nabla f(x)) + \frac{m}{2}\|\frac{1}{m}\nabla f(x)\|^{2} \\
    &= f(x) - \frac{1}{2m}\|\nabla f(x)\|^{2}.
\end{array}
$$

By rearrangement, we obtain

$$
\|\nabla f(x)\|^{2} \ge 2m \left[f(x) - f(x^{\ast})\right].
$$

If $\|\|\nabla f(x)\|\| < \delta$, we have

$$
f(x) - f(x^{\ast}) \le \frac{\|\nabla f(x)\|^{2}}{2m} \le \frac{\delta^{2}}{2m}.
$$

Thus, we can conclude when the gradient is small, we are close to having found a point with minimal function value.

Besides the function value, we can also derive an estimate of the distance of $x$ to the optimal point $x^{\ast}$ in terms of gradient,

$$
\begin{array}{ll}
f(x^{\ast}) &\ge f(x) + \nabla f(x)^{\top}(x^{\ast} - x) + \frac{m}{2}\|x^{\ast} - x\|^{2} \\
& \ge f(x) - \underbrace{\|\nabla f(x)\| \|x^{\ast}\ - x\|}_{\mathrm{cauchy-schwarz:}(\langle a, b\rangle)^{2} \le \|a\|^{2}\|b\|^{2}} + \frac{m}{2}\|x^{\ast} - x\|^{2}.
\end{array}
$$

By rearrangement, we obtain

$$
\frac{m}{2}\|x^{\ast} - x\|^{2} \le \|\nabla f(x)\| \|x^{\ast}\ - x\| + \underbrace{(f(x)^{\ast} - f(x))}_{\le 0} \rightarrow \|x^{\ast} - x\| \le \frac{2}{m}\|\nabla f(x)\|.
$$

With the above derivations, we can now analyze the convergence of the steepest-descent method on strongly convex functions.

$$
\begin{array}{ll}
f(x_{k+1})  = f(x_{k} - \frac{1}{L}\nabla f(x_{k})) &\le f(x_{k})-\frac{1}{2L}\|\nabla f(x_{k})\|^{2} \\
&\le f(x_{k})-\frac{m}{L}(f(x_{k}) - f(x^{\ast})).
\end{array}
$$

By some simple math, we can transform the above inequality into,

$$
f(x_{k + 1}) - f(x^{\ast}) \le (1 - \frac{m}{L})(f(x_{k}) - f(x^{\ast})).
$$

And by the recursion, we can finally obtain

$$
f(x_{T}) - f(x^{\ast}) \le (1 - \frac{m}{L})^{T}(f(x_{0}) - f(x^{\ast})).
$$

Thus, we can conclude the sequence of the function values _<span style="color:red">converges linearly</span>_ to the optimum.

#### Comparison about rates




[1]: https://icourse.club/uploads/files/bd85e2cdfb9463ca73fb2245b0f6097b3803b6e6.pdf