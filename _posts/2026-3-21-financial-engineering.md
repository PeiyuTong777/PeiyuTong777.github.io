---
title: "三叉树定价模型的定价唯一性缺失"
date: 2026-03-21
permalink: /posts/2026/03/financial-engineering/
mathjax: true
tags:
  - Financial Engineering
  - Asset Pricing
---

本文旨在通过资产定价第一、第二基本定理，证明在仅由股票和债券构成的三叉树模型中，无法为期权提供唯一的无套利定价。

---

## 一. 市场模型设定

考虑一个只含股票、债券、期权的单期离散金融市场，期初时间 $t=0$，期末时间 $t=1$。无风险债券期初价值为 $B_0 = 1$，无风险收益率为 $r$，股票期初价值为 $S_0$。市场不存在套利机会。

构建三叉树模型如下：
- 期末市场的状态空间包含三种不同的状态 $\Omega = \lbrace \omega_1, \omega_2, \omega_3 \rbrace$，分别对应三种不同的变动率 $u, m, d$。
- 期末债券在不同状态下的价值为 $B_1(\omega_1) = B_1(\omega_2) = B_1(\omega_3) = e^{r\delta t}$。
- 期末股票在不同状态下的价值为 $S_1(\omega_1) = S_0u$，$S_1(\omega_2) = S_0m$，$S_1(\omega_3) = S_0d$。
- 设期权初始价格为 $f_0$，期末价值为 $f_1(\omega_i)$，$i = 1, 2, 3$。

期权的期末价值 $f_1$ 确定如下（其中 $K$ 为行权价）：
$$
f_1 = \begin{cases} 
\max\lbrace S_1 - K, 0 \rbrace & \text{(看涨期权)} \\ 
\max\lbrace K - S_1, 0 \rbrace & \text{(看跌期权)} 
\end{cases}
$$

## 二. 证明

### 1. 寻找风险中性概率测度
根据**资产定价第一基本定理 (1st FTAP)**：市场无套利当且仅当存在一个风险中性概率测度 $\mathbb{Q} = (q_u, q_m, q_d)$。由此得到线性方程组：

$$
\begin{cases} 
q_u + q_m + q_d = 1 \\ 
e^{-r\delta t}(q_u S_1(\omega_1) + q_m S_1(\omega_2) + q_d S_1(\omega_3)) = S_0
\end{cases}
$$

约束条件为：$q_u, q_m, q_d > 0$。将股票期末价值代入整理得：

$$
\begin{cases} 
q_u + q_m + q_d = 1 \\ 
q_u u + q_m m + q_d d = e^{r\delta t}
\end{cases}
$$

### 2. 测度的非唯一性
将上述方程组改写为矩阵形式 $A \mathbf{q} = \mathbf{b}$：

$$
\begin{pmatrix} 1 & 1 & 1 \\ u & m & d \end{pmatrix} 
\begin{pmatrix} q_u \\ q_m \\ q_d \end{pmatrix} = 
\begin{pmatrix} 1 \\ e^{r\delta t} \end{pmatrix}
$$

对于系数矩阵 $A$，由于 $u, m, d$ 互不相等，故其秩 $\text{rank}(A) = 2$。由于未知数个数 $n=3$ 大于方程个数，该线性系统有无穷多个解。

### 3. 非完备市场
根据**资产定价第二基本定理 (2nd FTAP)**：一个无套利市场是完备的当且仅当存在**唯一**的风险中性测度。

我们已经证明了 $\mathbb{Q}$ 具有无穷多解，因此三叉树模型下的市场是**不完备**的。这意味着期权无法被基础资产（股票和债券）的组合完美复制。

### 4. 唯一定价的失效
考虑风险中性定价公式：

$$
f_0 = e^{-r\delta t} \mathbb{E}^{\mathbb{Q}}[f_1]
$$

展开为：
$$
f_0 = e^{-r\delta t} [q_u f_1(\omega_1) + q_m f_1(\omega_2) + q_d f_1(\omega_3)]
$$

由于存在无数个合法的 $\mathbb{Q}$，每一个测度都会对应一个不同的 $f_0$。这些 $f_0$ 构成了一个连续的价格区间，而非确定的唯一值。

## 三. 结论

在三叉树定价模型中，由于状态数（3）大于基础资产数（2），市场无法实现完备化。因为无法通过基础资产锁定唯一的复制成本，导致不存在一个单一、确定的无套利价格。
