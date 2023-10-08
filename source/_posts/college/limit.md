---
title: 关于无穷小主部的求法
shortlink: limit-mainitem
date: 2023-10
tags: college-math limit
mathjax: true
---
## I. 基础：待定系数法

<!--more-->
简单而言，就是用主部的形式写下来解方程:
$$
\lim_{x \rightarrow {x_0}}\cfrac{f(x)}{c\cdot x^p} = 1
$$
原理很简单，但是过程挺麻烦，适合用来直接证明主项。有时候用洛必达法则也会变简单（但是为什么不泰勒展开呢？）

## II. 通用方法：泰勒展开

结论放在这里，证明过程先咕咕了
$$
设有f(x)=\sum r_ix^{p_i} \ \ (r_i \in R^{\star})
$$

则记主项为
$$
o(f(x))=r_1x^{p_1}
$$
一句话就是第一个非零的展开项。

## III. 常用结论

泰勒展开也巨慢了，还容易错，下面是些结论。

- $o(1-\cos{x}) = \cfrac{x^2}2$

- $o((1+x)^r-1) = rx$

- $o(e^x - 1) = x$

- $o(ln(1+x)) = x$

- $o(\arcsin{x}) = x$

- $o(\arctan{x}) = x$

注意里面的x全部可以被替换为任意无穷小量。
