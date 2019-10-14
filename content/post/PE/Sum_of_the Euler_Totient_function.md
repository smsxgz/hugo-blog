---
title: "Sum of the Euler Totient function"
date: 2019-08-26
lastmod: 2019-08-26
tags: ['Algorithm', 'Project Euler']
mathjaxEnableAutoNumber: true
---
In this article, we introduce an algorithms for computing sum of the Euler Totient function. 
Our approach completely comes from the approach in the [previous article](https://smsxgz.github.io/post/pe/counting_square_free_numbers/) about counting square free numbers.

The Euler totient function $\varphi(n)$ is defined as the number of positive integers less than $n$ which are co-prime with $n$. Our goal is to compute the sum of all values of the totient function up to a certain $N$

<div>$$
S(N) = \sum_{n=1}^N \varphi(n).
$$</div>

<!--more-->
## 1. Establishing our formula
Note that 

<div>$$
\varphi(n) = \sum_{d \vert n} \mu(d) \frac{n}{d},
$$</div>

where $\mu(d)$ is the [Mobius function](https://en.wikipedia.org/wiki/M%C3%B6bius_function).  
Thus we can rewrite $S(N)$ as

<div style="font-size: 90%;">$$\begin{align}
\notag S(N) &= \sum_{n=1}^N \sum_{d \vert n} \mu(d) \frac{n}{d} \\
\notag &= \sum_{d=1}^N \frac{\mu(d)}{d} \sum_{d \vert n, n \le N} n \\
\notag &= \sum_{d=1}^N \mu(d) \sum_{k \le \frac{N}{d}} k \\
&= \frac{1}{2} \sum_{d=1}^N \mu(d) \left\lfloor \frac{N}{d} \right\rfloor \left( \left\lfloor \frac{N}{d} \right\rfloor + 1 \right)
\end{align}$$</div>

For sufficiently large $D$, the value of $\left\lfloor \frac{N}{d} \right\rfloor$ with respect to
$d > D$ will not change frequently. Based on this observation, we break the sum in (1) into two part:
$$S(N) = S_1(N) + S_2(N),$$
where

<div>$$\begin{align*}
S_1(N) &= \frac{1}{2} \sum_{1 \le d \le D}\mu(d)\left\lfloor \frac{N}{d} \right\rfloor \left( \left\lfloor \frac{N}{d} \right\rfloor + 1 \right), \\
S_2(N) &= \frac{1}{2} \sum_{d > D}\mu(d)\left\lfloor \frac{N}{d} \right\rfloor \left( \left\lfloor \frac{N}{d} \right\rfloor + 1 \right).
\end{align*}$$</div>

Rewrite the sum $S_2(N)$ as:

<div>$$\begin{align*}
S_2(N) &= \frac{1}{2} \sum_{i} i(i+1) \sum_{d > D, \left\lfloor \frac{N}{d} \right\rfloor = i} \mu(d) \\
&= \frac{1}{2} \sum_{i} i(i+1) \sum_{d > D, \frac{N}{i+1} < d \le \frac{N}{i}} \mu(d).
\end{align*}$$</div>

Let $x_i = \left\lfloor \frac{N}{i} \right\rfloor$, $i \le I \le \sqrt{N}$ and $D = x_I$.
Now the sum $S_2(N)$ can be simplified to:

<div style="font-size: 90%;">$$\begin{align}
\notag S_2(N) &= \frac{1}{2} \sum_{i=1}^{I-1} i(i+1) \sum_{x_{i+1} < d \le x_i} \mu(d) \\
\notag &= \frac{1}{2} \sum_{i=1}^{I-1} i(i+1) (M(x_i) - M(x_{i+1})) \\
&= \sum_{i=1}^{I-1} i M(x_i) - \frac{I(I - 1)}{2}M(x_I),
\end{align}$$</div>

where $M(x)$ is the [Mertens function](https://en.wikipedia.org/wiki/Mertens_function), i.e.,
$M(x) = \sum_{i=1}^{\lfloor x \rfloor}\mu(i)$.

## 2. Computing Mertens function
Applying the [Mobius inversion formula](https://en.wikipedia.org/wiki/M%C3%B6bius_inversion_formula), we know that $\sum_{d=1}^{x} M\left(\frac{x}{d}\right) = 1$, i.e.,

$$M(x) = 1 - \sum_{d=2}^{x} M\left( \frac{x}{d} \right).$$

Here, an important observation is that having all values $M(x/d)$ for $d \ge 2$, we
are able to calculate $M(x)$ in time $\mathcal{O}(\sqrt{x})$.
This is because there are at most $2\sqrt{x}$ different integers of the form $\lfloor x/d \rfloor$.

Moreover, to compute $M(x_i)$ we need the values $M(x_i / d)$ for $d \ge 2$.
If $x_i / d \le D$ then $M(x_i/d)$ was determined during the computation of $S_1(n)$.
If $x_i / d > D$ then
$$\left\lfloor \frac{x_i}{d} \right\rfloor
= \left\lfloor \frac{\left\lfloor \frac{N}{i} \right\rfloor}{d} \right\rfloor
= \left\lfloor \frac{N}{i d} \right\rfloor,$$
thus $M(x_i / d) = M(x_j)$ for $j = i d < I$.

It worth noting that it is important to compute $M(x_i)$ in a decreasing order.

## 3. The code of our algorithm
```python
def euler_sum(N, Imax):
    D = N // Imax

    # compute S1
    prime = [1] * (D + 1)
    mobius = [1] * (D + 1)

    for i in range(2, D + 1):
        if not prime[i]:
            continue
        mobius[i] = -1
        for j in range(2, D // i + 1):
            prime[i * j] = 0
            mobius[i * j] *= -1
        for j in range(1, D // (i * i) + 1):
            mobius[j * i * i] = 0

    s1 = 0
    for i in range(1, D + 1):
        k = N // i
        s1 += mobius[i] * (k * (k + 1) // 2)

    # compute M(d), d = 1, ..., D
    M_list = [0]
    M = 0
    for m in mobius[1:]:
        M += m
        M_list.append(M)

    # compute M(n // i), i = Imax - 1, ..., 1
    Mxi_list = []
    Mxi_sum = 0
    for i in range(Imax - 1, 0, -1):
        Mxi = 1
        xi = N // i

        sqd = int(sqrt(xi))
        # we know that sqd < D <= xi
        for j in range(1, xi // (sqd + 1) + 1):
            Mxi -= (xi // j - xi // (j + 1)) * M_list[j]

        for j in range(2, sqd + 1):
            if xi // j <= D:
                Mxi -= M_list[xi // j]
            else:
                Mxi -= Mxi_list[Imax - j * i - 1]

        Mxi_list.append(Mxi)
        Mxi_sum += i * Mxi

    # compute S2
    s2 = Mxi_sum - Imax * (Imax - 1) // 2 * M_list[-1]
    return s1 + s2
```

## 4. The complexity
Let us estimate the time complexity of above algorithm.
Computing $S_1(n)$ has time complexity $\mathcal{O}(D \log\log D).$
The computation of $S_2(n)$ is dominated by the loop for computing $M(x_i)$.
Therefore computing $S_2(n)$ has the time complexity:

<div>$$
\sum_{i=1}^{I} \mathcal{O}(\sqrt{x_i}) = \sum_{i=1}^{I} \mathcal{O}\left(\sqrt{\frac{N}{i}}\right)
= \mathcal{O} \left(\sqrt{N} \sum_{i=1}^{I} \frac{1}{\sqrt{i}} \right)
= \mathcal{O}(N^{1/2}I^{1/2}).
$$</div>

due to computing $M(x_i)$ takes $\mathcal{O}(\sqrt{x_i})$ time.

If we choose $I = N^{1/3}$, then

<div>$$\begin{align*}
\mathcal{O}(N^{1/2}I^{1/2}) &= \mathcal{O}(N^{2/3}), \text{ and }\\
\mathcal{O}(D \log\log D) &= \tilde{\mathcal{O}}\left(\frac{N}{I}\right)
= \tilde{\mathcal{O}}(N^{2/3}).
\end{align*}$$</div>

So the optimal total complexity is $\tilde{\mathcal{O}}(N^{2/3})$.

## 5. Another algorithm from [Beni Bogoşel's blog](https://mathproblems123.wordpress.com/2018/05/10/sum-of-the-euler-totient-function)
Applying the Mobius inversion formula to the Equation (1), we have

<div>$$\begin{align*}
\frac{N(N+1)}{2} = \sum_{d=1}^N S\left( \left\lfloor\frac{N}{d}\right\rfloor \right),
\end{align*}$$</div>

that is

<div style="font-size: 90%;">$$\begin{align}
S(N) = \frac{N(N+1)}{2} - \sum_{d=2}^N S\left( \left\lfloor\frac{N}{d}\right\rfloor \right).
\end{align}$$</div>

As we did before, let $x_i = \left\lfloor \frac{N}{i} \right\rfloor$, $2 \le i \le I \le \sqrt{N}$ and $D = x_I$.  
Rewrite Equation (3) as

<div style="font-size: 90%;">$$\begin{align}
S(N) = \frac{N(N+1)}{2} - \sum_{d=2}^{I-1} S(x_i) - \sum_{k=1}^D \left( \left\lfloor \frac{N}{k} \right\rfloor - \left\lfloor \frac{N}{k+1} \right\rfloor \right) S(k),
\end{align}$$</div>

Moreover, to compute $S(x_i)$ we need the values $S(x_i / d)$ for $d \ge 2$.  
If $x_i / d \le D$ then $S(x_i/d)$ was determined during the computation of last part of (4).
If $x_i / d > D$ then $S(x_i / d) = S(x_j)$ for $j = i d < I$.

Same as our algorithm, the complexity of above algorithm is $\mathcal{O}(\sqrt{NI}) + \tilde{\mathcal{O}}(N/I)$, which can achieve optimal magnitude $\tilde{\mathcal{O}}(N^{2/3})$ by taking $I = N^{1/3}$.

## Reference
1. Sum of the Euler Totient function [_Beni Bogoşel's blog_](https://mathproblems123.wordpress.com/2018/05/10/sum-of-the-euler-totient-function)