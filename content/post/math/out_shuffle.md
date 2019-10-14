---
title: "Period of out-shuffle"
date: 2019-08-28
lastmod: 2019-08-29
tags: ['Math', 'Card Games']

mathjaxEnableAutoNumber: true
---

An out-shuffle is a riffle shuffle in which the top half of the deck is taken in the left hand and cards are then alternatively interleaved from left and right hands with preserving the location of the top and bottom card of the deck.

Suppose a deck of size $6$ originally arranged as $(1, 2, 3, 4, 5, 6)$. After an out-shuffle, the deck will become $(1, 4, 2, 5, 3, 6)$.

{{% admonition type="tip" title="Period of out-shuffle" %}}
Let $n$ is an even integer and $s(n)$ be the minimum number of out-shuffles needed to restore a deck of size $n$ to its original order. Then we have

<div>$$\begin{equation*}
s(n) = \min_{m} \{ m : 2^m \equiv 1 (\bmod n - 1)  \}.
\end{equation*}$$</div>

{{% /admonition %}}

<!--more-->

In fact, $s(6) = 4$. In particular,  
$(1,2,3,4,5,6) \rightarrow (1,4,2,5,3,6) \rightarrow (1,5,4,3,2,6) \rightarrow (1,3,5,2,4,6) \rightarrow (1,2,3,4,5,6)$

It is clearly that the out-shuffle can be viewed as a permutation $\sigma_n \in S_n$, where $S_n$ is the symmetric group on $n$ letters and

<div>$$\begin{align*}
\sigma_n (i) = 
\begin{cases}
2 i - 1, &\text{ for } i \le n/2, \\
2 i - n, &\text{ for } n/2 < i \le n. 
\end{cases}
\end{align*}$$</div>

<!-- Then, $s(n)$ is equal to the order of $\sigma_n$. In particular, $\sigma_6 = (1) (2~3~5~4) (6)$, and the order of $\sigma_6$ is $4$ apparently. -->

The key observation is that $\sigma_n(i) \equiv 2i - 1 (\bmod n - 1)$. Thus $i = \sigma_n^k (i)$ for $2 \le i \le n-1$ is equivalent to

<div>$$\begin{align*}
i = \sigma_n^k (i) \equiv 2^{k} (i-1) + 1 (\bmod n - 1), \text{ for } 2 \le i \le n-1,
\end{align*}$$</div>

that is 

<div>$$\begin{align*}
(2^{k} - 1) (i-1) &\equiv 0 (\bmod n - 1), \text{ for } 2 \le i \le n-1, \text{ or } \\
2^{k} &\equiv 1 (\bmod n - 1).
\end{align*}$$</div>

<p align="right">☐</p>

__Reference:__  
[1] [Out-Shuffle (mathworld)](http://mathworld.wolfram.com/Out-Shuffle.html)

