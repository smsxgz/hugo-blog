---
title: "Interesting problems from AMM"
date: 2019-03-05
lastmod: 2019-03-05
tags: ['problem set', 'AMM']
---
Following are some interesting problems from _The American Mathematical Monthly_.
<!--more-->
### Problem 1
See https://www.tandfonline.com/doi/pdf/10.1080/00029890.2018.1470415.  

Let $f$ be a $C^1$ real-valued function on $[0, 1]$, infinitely differentiable at $x=0$, and such that $f^{(n)}(0) = 0$ for every $n \in \mathbb{N}$. If

<div>$$
|xf'(x)| \le C \cdot |f(x)| \text{ for some } C > 0 \text{ and every } x \in [0, 1],
$$</div>

then $f(x) = 0$ for every $x \in [0, 1]$.

__*Hint:*__
Use

<div>$$
(x^{-a-1}f^2(x))' = -(a+1)x^{-a-2}f^2(x) + 2x^{-a-1}f(x)f'(x)
$$</div>

<br>

### Problem 2
See https://www.tandfonline.com/doi/pdf/10.1080/00029890.2018.1507205.

Let $D$ be a convex subset of a real vector space, and $f: D\rightarrow \mathbb{R} \cup \\{+\infty\\}$ be a radially lower semicontinuous function, that is,
$$f(x) \le \liminf\limits_{t \rightarrow 0} f(x + t (y - x)).$$
Then $f$ is convex iff for all $x, y \in D$, there exsits $\lambda = \lambda(x, y) \in (0, 1)$ that satisfies inequality
$$f(\lambda x + (1-\lambda) y) \le \lambda f(x) + (1-\lambda) f(y).$$

__*Hint:*__
Fix $x, y \in D$ and define the set
$$ S \triangleq \\{\lambda \in [0,1]: f(\lambda x + (1-\lambda) y) \le \lambda f(x) + (1-\lambda) f(y)\\}. $$
Prove that  
(a) $S$ is a nonempty compact subset of $[0, 1]$.  
(b) $(\lambda_1, \lambda_2) \cap S \neq \emptyset$ for all $\lambda_1, \lambda_2 \in S$ with $\lambda_1 < \lambda_2$.

<br>

### Problem 3
See https://www.tandfonline.com/doi/pdf/10.1080/00029890.2019.1537761.  

Let $\\{ x_n \\}$ be a positive nonincreasing real sequence such that

<div>$$\sum_n x_n = +\infty, ~~~~ \lim_{n \rightarrow \infty} x_n = 0.$$</div>

Denote

<div>$$L \triangleq \liminf_{n \rightarrow \infty} \frac{x_{n+1}}{x_n}.$$</div>

Prove that  
(a) if $L > 1/2$, then there exists a constant $\theta \in (0, 1)$ such that, for each $l > 0$, there is a subsequence $\\{ x_{n_k} \\}$ for which

<div>$$\sum_k x_{n_k} = l \text{  and  } x_{n_k} = \mathcal{O}(\theta^k).$$</div>

(b) if $L > \frac{\sqrt{5} - 1}{2}$, then for each $l > 0$ and for each $\epsilon > 0$, there exists a subsequence $\\{x_{n_k}\\}$ for which

<div>$$\sum_k x_{n_k} = l \text{  and  } x_{n_k} = \mathcal{O}(\sqrt{1 + \epsilon - L}^k).$$</div>



### Reference
[1] George Stoica (2018) When Must a Flat Function be Identically 0?, The
American Mathematical Monthly, 125:7, 648-649, DOI: 10.1080/00029890.2018.1470415  
[2]  Paolo Leonetti (2018) A Characterization of Convex Functions, The American
Mathematical Monthly, 125:9, 842-844, DOI: 10.1080/00029890.2018.1507205  
[3] Paolo Leonetti (2019) Convergence Rates of Subseries, The American
Mathematical Monthly, 126:2, 163-167, DOI: 10.1080/00029890.2019.1537761  
