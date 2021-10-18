---
title: FWT变换
shortlink: fwt
tags: math
mathjax: true
---

# FWT变换

本文参考 <https://codeforces.com/blog/entry/96003>

## FWT

考虑两个数组 $A$ $B$ ，求解数组 $C$ 有
$$
C_k = \sum_{i*j=k}A_i B_j
$$
<!--more-->
我们考察 $*$ 为 $\oplus$，关注最简单的情况，即只有一位的情况：
我们可以构造魔法矩阵 $T$ 进行变换:
$$
T=\left(\begin{matrix}
a & b\\
c & d\\
\end{matrix}\right)
$$

$$
T \left( \begin{matrix} p_0 \\ p_1\end{matrix}\right) \cdot
T\left( \begin{matrix} p_0 \\ p_1\end{matrix}\right) =
T\left( \begin{matrix} p_0q_0+p_1q_1 \\ p_0q_1 + p_1q_0\end{matrix}\right)
$$

$$
\Big( \matrix{ a^2p_0q_0+abp_0q_1+abp_1q_0+b^2p_1q_1 \\ c^2p_0q_0+cdp_0q_1+cdp_1q_0+d^2p_1q_1} \Big)=\Big( \matrix{ ap_0q_0+bp_0q_1+bp_1q_0+ap_1q_1 \\ cp_0q_0+dp_0q_1+dp_1q_0+cp_1q_1} \Big)
$$

解方程可得： $a^2=a,ab=b,b^2=a$， 发现：$(a,b)={(0,0),(1,1),(1,-1)}$

$c, d$ 同理

考虑到在运算中 $T$ 应当可逆，则可以解得有:  $T=\Big( \matrix{1 & 1 \\ 1& -1} \Big)$ ， 且 $T^{-1}=\frac 12 \Big( \matrix{1 & 1 \\ 1& -1} \Big)$ .

我们考虑拓展，有记对于长度为2的情况为 $T_0$

长度为 $2^k$ 记作 $T_{k - 1}$

则有 $T_k = T_{k - 1} \otimes T_0$， 其中 $\otimes$ 为 Kronecker 积

考虑优化 $T_{k - 1}v$ 。

## Kronecker 积 [^1]

发现如下性质:

设 $v_0, v_1$ 为向量
$$
(I_2 \otimes T)\left( \matrix{v_0\\v_1} \right) = \left( \matrix{v_0T\\v_1T} \right)
$$

$$
(T \otimes I_2)\left( \matrix{v_0\\v_1} \right) = \left( \matrix{T_{11}v_0 +T_{12}v_1\\ T_{21}v_0 +T_{22}v_1} \right)
$$

同时借助混合乘积性质，有如下推论：
$$
A \otimes B \otimes C = (A \otimes I_n \otimes I_n)(I_n \otimes B \otimes I_n)(I_n \otimes I_n \otimes C)=(I_n \otimes I_n \otimes C)(I_n \otimes B \otimes I_n)(A \otimes I_n \otimes I_n)
$$

$$
((A \otimes I_n \otimes I_n)(I_n \otimes B \otimes I_n)(I_n \otimes I_n \otimes C))^{-1}=(A \otimes I_n \otimes I_n)^{-1}(I_n \otimes B \otimes I_n)^{-1}(I_n \otimes I_n \otimes C)^{-1}
$$

则我们的运算可以进行分治简化
$$
T_k \left( \matrix{v_0\\v_1}\right) = \left( \matrix{T_{11}T_{k-1}v_0 +T_{12}T_{k -1}v_1\\ T_{21}T_{k-1}v_0 +T_{22}T_{k -1}v_1\\} \right)
$$
即 FWT 的优化机理

[^1]: https://baike.baidu.com/item/%E5%85%8B%E7%BD%97%E5%86%85%E5%85%8B%E7%A7%AF/6282573