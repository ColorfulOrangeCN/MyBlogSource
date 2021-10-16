---
title: 符号化组合
shortlink: charcomb
date: 2021-10-10 21:36:32
tags: math
mathjax: true
---

## 组合类

组合类是代数结构 $\mathcal{C}=(\mathcal{S}, f)$ 其中 $f$ 是 $\mathcal{S}\rarr \mathbb{N}$ 的一个大小函数。

<!--more-->
对于组合类我们定义其的和与直积：

和：
$$
(\mathcal{S}_1, f_1) + (\mathcal{S}_2, f_2) = (\mathcal{S}_1 + \mathcal{S}_2, g)\\
其中：
g(x) = \begin{cases}
f_1(x) && x \in \mathcal{S}_1 \\
f_2(x) && x \in \mathcal{S}_1
\end{cases}
$$
直积：
$$
(\mathcal{S}_1, f_1) \times (\mathcal{S}_2, f_2) = (\mathcal{S}_1 \times \mathcal{S}_2, g)\\
其中：
g((x_1, x_2)) = f_1(x_1) + f_2(x_2)
$$

## 组合类与生成函数

我们定义组合类 $\mathcal{C}$ 的 OGF $G(\mathcal{C})$：
$$ {2}
G(\mathcal{C};z) = \sum_{x \in \mathcal{C}} z^{f(x)}
$$
则有:

组合类的直积对应生成函数的积，组合类的和对应生成函数的和。

显然我们发现有标号组合类 $C$

## 组合类的更多构造

### $\operatorname{SEQ}$ 运算:

$$
\operatorname{SEQ}(C)=\epsilon + C + C\times C + C\times C\times C + \cdots
$$

即任意多组合类$C$组合在一起的结果。

我们有
$$
B=\operatorname{SEQ}(C)\\
B(z)=\cfrac{1}{1-C(z)}
$$
