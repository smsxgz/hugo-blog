---
title: "Counting square free numbers"
date: 2019-02-12
lastmod: 2019-02-13
draft: true
tags: ['Algorithm', 'Project Euler']

mathjax: true
mathjaxEnableSingleDollar: true
---
In this article, we introduce an algorithm proposed by Jakub Pawlewicz [[1]](https://arxiv.org/pdf/1107.4890.pdf) to count square free numbers not exceeding $n$ efficiently.

We first introduce a well-known algorithm with time complexity $\tilde{\mathcal{O}}(\sqrt{n})$.
After that, we present the algorithm with time complexity $\tilde{\mathcal{O}}(n^{2/5})$ following from [[1]](https://arxiv.org/pdf/1107.4890.pdf).
To our knowledge, this algorithm is a kind of divide-and-conquer algorithm. This methodology is common in number theory problems in [Project Euler](https://projecteuler.net),
and we show the power of this methodology by two examples at last.
<!-- more -->
## Definition
