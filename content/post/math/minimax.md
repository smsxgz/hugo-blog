---
title: "Proof for Minimax Theorem"
date: 2019-03-24
lastmod: 2019-03-24
tags: ['math']

mathjaxEnableAutoNumber: false
---
{{% admonition type="tip" title="Von Neumann's minimax theorem (1928)" %}}
Let $X$ and $Y$ be compact convex sets. Suppose that $f:X \times Y \rightarrow \mathbb{R}$ is a continuous function that satisfies:  
1. $f(\cdot, y): X \rightarrow \mathbb{R}$ is convex for any fixed $y \in Y$,  
2. $f(x, \cdot): Y \rightarrow \mathbb{R}$ is concave for any fixed $x \in X$.  
Then, we have that

<div>$$\begin{equation}
\min_{x \in X} \max_{y \in Y} f(x, y) = \max_{y \in Y} \min_{x \in X} f(x, y)
\end{equation}$$</div>
{{% /admonition %}}
<br>
Following proof is from Hidetoshi Komiya [1].
<!--more-->
{{% admonition type="tip" title="Lemma" %}}
Under the same assumption of Von Neumann's theorem, for any finite $y_1, \cdots, y_n \in Y$ and any real number $\alpha$ with

<div>$$\alpha < \min_{x \in X} \max_{1 \le i \le n}f(x, y_i),$$</div>

there is $y_0 \in Y$ with $\alpha < \min f(x, y_0)$.
{{% /admonition %}}

__*Proof of lemma:*__  
<!-- We just need to prove this on the case $n = 2$.   -->
We prove this lemma by induction on $n$.  
In fact, lemma is trivial for $n = 1$. For $n \ge 2$, let $X'$ be the compact convex set $\\{ x \in X, f(x, y_n) \le \alpha \\}$. We may assume that $X'$ is nonempty, otherwise we take $y_0 = y_n$.  
Since

<div>$$\alpha < \min_{x \in X} \max_{1 \le i \le n}f(x, y_i),$$</div>
we have

<div>$$\alpha < \min_{x \in X'} \max_{1 \le i \le n-1}f(x, y_i).$$</div>

Apply induction-assumption to the restriction of $f$ to $X' \times Y$. Then there is $y_0'$ with

$$\alpha < \min_{x \in X'} f(x, y_0'),$$
thus

<div>$$\alpha < \min_{x \in X} \max \{f(x, y_0'), f(x, y_n)\}.$$</div>

Suppose that $\alpha \ge \min_{x \in X}f(x, y)$ for all $y \in Y$.  
Choose $\beta$ with

<div>$$\alpha < \beta < \min_{x \in X} \max \{f(x, y_0'), f(x, y_n)\}.$$</div>

Denote by $S$ the line segment joining $y_0'$ and $y_n$, and for each $z \in S$, define

<div>$$Cz = \{ x \in X: f(x, z) \le \alpha \}, ~~~ C'z = \{ x \in X: f(x, z) \le \beta \}.$$</div>

Then $Cz$ and $C'z$ are nonempty, closed and convex by the continuity and convexity of $f(\cdot, z)$.  
Denote $A = C'y_0'$ and $B = C'y_n$. Note that $A \cap B = \emptyset$. By the concavity of $f(x, \cdot)$,

<div>$$f(x, z) \ge \min\{ f(x, y_0'), f(x, y_n) \}$$</div>

for $x \in X$ and $z \in S$. Hence we have $C'z \subseteq A \cup B$. Since $C'z$ is convex, we have $C'z$ is connected, and hence either $Cz \subseteq C'z \subseteq A$ or $Cz \subseteq C'z \subseteq B$ hosts.  
Define

<div>$$ I = \{z \in S: Cz \subseteq A\} \text{ and } J = \{z \in S: Cz \subseteq B\}, $$</div>

then $I \cap J = \emptyset$ and $I \cup J = S$. Since $y_0' \in I$ and $y_n \in J$, $I$ and $J$ are nonempty.  
Let $\\{z_n\\}$ be a sequence in $I$ with $\lim z_n = z \in S$.
Let $x$ be any point of $Cz$. Then we have $f(x, z) < \beta$. By the continuity of $f(x, \cdot)$, we have

<div>$$ \lim_{n \rightarrow \infty} f(x, z_n) < \beta. $$</div>

Hence there is an integer $m$ with $f(x, z_m) < \beta$, that is, $x \in C'z_m$. We have $C'z_m \subseteq A$ according to $Cz_m \subseteq A$, and thus $x \in A$. That is, $z \in I$ and $I$ is closed in $S$. For the same reason, $J$ is also closed. The closedness of both $I$ and $J$ contradicts the connectedness of S.
<p align="right">☐</p>


__*Proof of the theorem:*__  
First note that $F(x) = \max_{y \in Y} f(x, y)$ is a convex function by

<div>$$\begin{align}
F(\lambda x_1 + (1 - \lambda) x_2) &= \max_{y \in Y} f(\lambda x_1 + (1 - \lambda) x_2, y) \\
&\le \max_{y \in Y} \lambda f(x_1, y) + (1 - \lambda) f(x_2, y) \\
&\le \lambda \max_{y \in Y} f(x_1, y) + (1 - \lambda) \max_{y \in Y} f(x_2, y) \\
&= \lambda F(x_1) + (1 - \lambda) F(x_2).
\end{align}$$</div>

Thus we can suppose that

<div>$$F(x_0) = \max_{x \in X} F(x),$$</div>

and we have

<div>$$
\min_{x \in X} \max_{y \in Y} f(x, y) = \max_{y \in Y} f(x_0, y) \ge \max_{y \in Y} \min_{x \in X} f(x, y).
$$</div>

On the other hand, let $\alpha$ be any number with

<div>$$\alpha < \min_{x \in X} \max_{y \in Y} f(x, y)$$</div>

and let $X_y$ be the compact set $\\{x \in X: f(x, y) \le \alpha\\}$ for each $y \in Y$.  
Then

<div>$$\bigcap_{y \in Y} X_y = \emptyset, $$</div>

and hence there are finite $y_1, \cdots, y_n \in Y$ such that

<div>$$\bigcap_{i=1}^n X_{y_i} = \emptyset,$$</div>

i.e.,

<div>$$\alpha < \min_{x \in X} \max_{1 \le i \le n} f(x, y_i).$$</div>

By above lemma, there is $y_0 \in Y$ with

<div>$$\alpha < \min_{x \in X} f(x, y_0) \le \max_{y \in Y} \min_{x \in X} f(x, y).$$</div>

Therefore we have

<div>$$
\min_{x \in X} \max_{y \in Y} f(x, y) \le \max_{y \in Y} \min_{x \in X} f(x, y).
$$</div>

<p align="right">☐</p>


__Reference:__  
[1] Komiya, H 1988, 'Elementary proof for sion's minimax theorem' Kodai Mathematical Journal, vol. 11, no. 1, pp. 5-7. https://doi.org/10.2996/kmj/1138038812
