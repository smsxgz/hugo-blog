---
title: "Counting square free numbers"
date: 2019-02-12
lastmod: 2019-02-14
tags: ['Algorithm', 'Project Euler']

mathjaxEnableAutoNumber: true
---
In this article, we introduce an algorithm proposed by Jakub Pawlewicz [[1]](#ref01) to count square free numbers not exceeding $n$ efficiently.

We first introduce a well-known algorithm with time complexity $\tilde{\mathcal{O}}(\sqrt{n})$.
After that, we present a algorithm which is a modified version of first algorithm with time complexity $\tilde{\mathcal{O}}(n^{2/5})$ following from [[1]](#ref01).

<!-- and can be viewed as a kind of divide-and-conquer algorithm. -->
<!-- Moreover, this methodology is common in number theory problems in [Project Euler](https://projecteuler.net),
and we show the power of this methodology by two examples at last. -->
<!--more-->
## 1. Definition
A square free number is an integer which is not divisible by a square of any integer greater than one. Apparently, an integer $d$ is a square free number iff $\mu(d) \neq 0$, where $\mu(d)$ is the [Mobius function](https://en.wikipedia.org/wiki/M%C3%B6bius_function).  
Let $S(n)$ denote the number of square free positive numbers less or equal to $n$, i.e.,
$$S(n) = \sum_{d=1}^{n} |\mu(d)|.$$
More information about square free numbers can be found at [[2]](#ref02), [[3]](#ref03).

## 2. The $\tilde{\mathcal{O}}(\sqrt{n})$ algorithm
### 2.1. Basic theorem
{{% admonition type="tip" title="Theorem 1." %}}
<div style="font-size: 90%;">$$\begin{equation}
S(n) = \sum_{d=1}^{\lfloor\sqrt{n}\rfloor}\mu(d)\left\lfloor\frac{n}{d^2}\right\rfloor.
\end{equation}$$</div>
{{% /admonition %}}
__*proof:*__  
Suppose $P = \\{p_1, p_2, \ldots, p_k\\}$ is the set which consists of all prime numbers less or equal to $n$.  
Let $A_i$ denote the set $\\{m \le n: p_i^2 \mid m \\}$.  
Thus

<div>$$
\begin{align*}
S(n) &= n - \left\vert \bigcup_{i=1}^k A_i \right\vert \\
&= n - \sum_{i=1}^{k} |A_i| + \sum_{1 \le i_1 < i_2 \le k} |A_{i_1} \cap A_{i_2}| \\
&\quad - \sum_{1 \le i_1 < i_2 < i_3 \le k} |A_{i_1} \cap A_{i_2} \cap A_{i_3}| + \ldots
+ (-1)^{k} \left\vert \bigcap_{i=1}^k A_i \right\vert.
\end{align*}
$$</div>

Note that

<div>$$
\left\vert \bigcap_{j=1}^l A_{i_j} \right\vert = \left\vert \left\{m \le n: \prod_{j=1}^l p_{i_j}^2 \mid m \right\} \right\vert = \left\lfloor \frac{n}{\prod_{j=1}^l p_{i_j}^2} \right\rfloor,
$$</div>

so

<div>$$\begin{align*}
S(n) &= n + \sum_{Q \subseteq P, Q \text{ is not empty }} (-1)^{|Q|} \left\lfloor \frac{n}{\prod_{p \in Q} p^2} \right\rfloor \\
&= \sum_{Q \subseteq P} \mu\left(\prod_{p \in Q} p\right) \left\lfloor \frac{n}{\prod_{p \in Q} p^2} \right\rfloor.
\end{align*}$$</div>

For $Q_1 \neq Q_2$, it is clearly that

<div>$$\prod_{p \in Q_1} p \neq \prod_{p \in Q_2} p,$$</div>

and any square free number less or equal to $n$ can be express as $\prod_{p \in Q} p$.  
Thus

<div>$$\begin{align*}
S(n) &= \sum_{d \le \sqrt{n}, d \text{ is square free }} \mu(d) \left\lfloor \frac{n}{d^2} \right\rfloor \\
&= \sum_{d=1}^{\lfloor \sqrt{n} \rfloor} \mu(d) \left\lfloor \frac{n}{d^2} \right\rfloor.
\end{align*}$$</div>

<p align="right">☐</p>

### 2.2. Computing Mobius function
Following Theorem 1, we need to find the values of $\mu(d)$ for $d = 1, \ldots, K$, where $K = \lfloor \sqrt{n} \rfloor$.
This can be done in time $\mathcal{O}(K \log\log K)$ using a sieve similar to the sieve of Eratosthenes. Following is my code in Python.
```python
def Mobius(n):
    prime = [1] * (n + 1)
    mobius = [1] * (n + 1)

    for i in range(2, n + 1):
        if not prime[i]:
            continue
        mobius[i] = -1
        for j in range(2, n // i + 1):
            prime[i * j] = 0
            mobius[i * j] *= -1
        for j in range(1, n // (i * i) + 1):
            mobius[j * i * i] = 0
    return mobius
```

### 2.3. The algorithm
Now, our first algorithm is obvious.
```python
def square_free(n):
    sq = int(sqrt(n))
    mobius = Mobius(sq)

    s = 0
    for i in range(1, sq + 1):
        s += mobius[i] * (n // (i * i))
    return s
```
Apparently, above algorithm has $\tilde{\mathcal{O}}(\sqrt{n})$ time complexity.

## 3. The modified algorithm
### 3.1. Establishing the new formula
For sufficiently large $D$, the value of $\left\lfloor \frac{n}{d^2} \right\rfloor$ with respect to
$d > D$ will not change frequently. Based on this observation, we break the sum in (1) into two part:
$$S(n) = S_1(n) + S_2(n),$$
where

<div>$$\begin{align*}
S_1(n) &= \sum_{1 \le d \le D}\mu(d)\left\lfloor \frac{n}{d^2} \right\rfloor, \\
S_2(n) &= \sum_{d > D}\mu(d)\left\lfloor \frac{n}{d^2} \right\rfloor.
\end{align*}$$</div>

Rewrite the sum $S_2(n)$ as:

<div>$$\begin{align*}
S_2(n) &= \sum_{d > D} \sum_i 1_{i = \left\lfloor \frac{n}{d^2} \right\rfloor} i \mu(d) \\
&= \sum_{i} i \sum_{d > D} \mu(d) 1_{i \le \frac{n}{d^2} < i+1} \\
&= \sum_{i} i \sum_{d > D, \left\lfloor \sqrt{\frac{n}{i+1}} \right\rfloor < d \le \left\lfloor \sqrt{\frac{n}{i}} \right\rfloor} \mu(d).
\end{align*}$$</div>

Observe that, for $i \le \sqrt[3]{\frac{n}{4}}$,

<div>$$\begin{align*}
\sqrt{\frac{n}{i - 1}} - \sqrt{\frac{n}{i}} &= \sqrt{n} \frac{\sqrt{i} - \sqrt{i-1}}{\sqrt{i}\sqrt{i-1}} \\
&\ge \frac{\sqrt{n}}{i} \frac{1}{\sqrt{i} + \sqrt{i-1}} \\
&\ge \sqrt{\frac{n}{4i^3}} \ge 1.
\end{align*}$$</div>

Let denote $x_i = \sqrt{\frac{n}{i}}$, take $I \le \sqrt[3]{\frac{n}{4}}$ and $D = x_I$.
Now the sum $S_2(n)$ can be simplified to:

<div style="font-size: 90%;">$$\begin{align}
\notag S_2(n) &= \sum_{i=1}^{I-1} i \sum_{x_{i+1} < d \le x_i} \mu(d) \\
\notag &= \sum_{i=1}^{I-1} i(M(x_i) - M(x_{i+1})) \\
&= \sum_{i=1}^{I-1} M(x_i) - (I - 1)M(x_I),
\end{align}$$</div>

where $M(x)$ is the [Mertens function](https://en.wikipedia.org/wiki/Mertens_function), i.e.,
$M(x) = \sum_{i=1}^{\lfloor x \rfloor}\mu(i)$.

### 3.2. Computing Mertens function
Applying the [Mobius inversion formula](https://en.wikipedia.org/wiki/M%C3%B6bius_inversion_formula), we know that $\sum_{d=1}^{x} M\left(\frac{x}{d}\right) = 1$, i.e.,

$$M(x) = 1 - \sum_{d=2}^{x} M\left( \frac{x}{d} \right).$$

Here, an important observation is that having all values $M(x/d)$ for $d \ge 2$, we
are able to calculate $M(x)$ in time $\mathcal{O}(\sqrt{x})$.
This is because there are at most $2\sqrt{x}$ different integers of the form $\lfloor x/d \rfloor$.

Moreover, to compute $M(x_i)$ we need the values $M(x_i / d)$ for $d \ge 2$.
If $x_i / d \le D$ then $M(x_i/d)$ was determined during the computation of $S_1(n)$.
If $x_i > D$ then
$$\left\lfloor \frac{x_i}{d} \right\rfloor
= \left\lfloor \frac{\left\lfloor \sqrt{\frac{n}{i}} \right\rfloor}{d} \right\rfloor
= \left\lfloor \frac{\sqrt{\frac{n}{i}}}{d} \right\rfloor
= \left\lfloor \sqrt{\frac{n}{d^2i}} \right\rfloor,$$
thus $M(x_i / d) = M(x_j)$ for $j = d^2 i$.
Of course $j < I$, because otherwise $\sqrt{n/j} \le D$.

It worth noting that it is important to compute $M(x_i)$ in a decreasing order.

### 3.2. The algorithm
```python
def efficient_square_free(N, Imax):
    D = int(sqrt(N / Imax))
    mobius = Mobius(D)

    # compute S1
    s1 = 0
    for i in range(1, D + 1):
        s1 += mobius[i] * (N // (i * i))

    # compute M(d), d = 1, ..., D
    M_list = [0]
    M = 0
    for m in mobius[1:]:
        M += m
        M_list.append(M)

    # compute M(sqrt(n / i)), i = Imax - 1, ..., 1
    Mxi_list = []
    Mxi_sum = 0
    for i in range(Imax - 1, 0, -1):
        Mxi = 1
        xi = int(sqrt(N // i))

        sqd = int(sqrt(xi))
        # sqd < D <= xi
        for j in range(1, xi // (sqd + 1) + 1):
            Mxi -= (xi // j - xi // (j + 1)) * M_list[j]

        for j in range(2, sqd + 1):
            if xi // j <= D:
                Mxi -= M_list[xi // j]
            else:
                Mxi -= Mxi_list[Imax - j * j * i - 1]

        Mxi_list.append(Mxi)
        Mxi_sum += Mxi

    # compute S2
    s2 = Mxi_sum - (Imax - 1) * M_list[-1]
    return s1 + s2
```

### 3.3. The complexity
Let us estimate the time complexity of above algorithm.
Computing $S_1(n)$ has time complexity $\mathcal{O}(D \log\log D).$
The computation of $S_2(n)$ is dominated by the loop for computing $M(x_i)$.
Therefore computing $S_2(n)$ has the time complexity:

<div>$$
\sum_{i=1}^{I} \mathcal{O}(\sqrt{x_i}) = \sum_{i=1}^{I} \mathcal{O}\left(\sqrt[4]{\frac{n}{i}}\right)
= \mathcal{O} \left(\sqrt[4]{n} \sum_{i=1}^{I} \frac{1}{\sqrt[4]{i}} \right)
= \mathcal{O}(n^{1/4}I^{3/4}).
$$</div>

due to computing $M(x_i)$ takes $\mathcal{O}(\sqrt{x_i})$ time.

If we choose $I = n^{1/5}$, then

<div>$$\begin{align*}
\mathcal{O}(n^{1/4}I^{3/4}) &= \mathcal{O}(n^{2/5}), \text{ and }\\
\mathcal{O}(D \log\log D) &= \tilde{\mathcal{O}}\left(\sqrt{\frac{n}{I}}\right)
= \tilde{\mathcal{O}}(n^{2/5}).
\end{align*}$$</div>

## 4. Results
Following table is my run time of above two algorithms for different $n$.
Both Python functions are accelerated by `numba.jit`.

| $n$           | square_free     | efficient_square_free|
| ------------- |:---------------:| -------------------- |
| 2**50         | 3.124           | 0.510                |
| 2**52         | 6.028           | 0.595                |
| 2**55         | 18.465          | 0.869                |


## Reference
1. <a id="ref01">Jakub Pawlewicz, _Counting Square-Free Numbers_, [arxiv: 1107.4890](https://arxiv.org/pdf/1107.4890.pdf)</a>
2. <a id="ref02">[Wikipedia: Square-free integer](https://en.wikipedia.org/wiki/Square-free_integer)</a>
3. <a id="red03">[Wolfram MathWorld: Squarefree](http://mathworld.wolfram.com/Squarefree.html)</a>
4. <a id="red04">[Squarefree Numbers (Project Euler)](https://projecteuler.net/problem=193)</a>