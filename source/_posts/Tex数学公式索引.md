---
title: Tex数学公式索引
date: 2020-03-06 20:54:53
tags:
  - 百宝箱
---
## Tex数学公式符号

### 希腊字母

ɛ \varepsilon
ϕ \varphi
α \alpha Alpha Α
β \beta Beta Β
γ \gamma Gamma Γ
δ \delta Delta Δ
ε \epsi Epsilon Ε
ζ \zeta Zeta Ζ
η \eta Eta Η
θ \theta Theta Θ
λ \lambda Lambda Λ
μ \mu Mu Μ
ξ \xi Xi Ξ
π \pi Pi Π
ρ \rho Rho Ρ
σ \sigma Sigma Σ
τ \tau Tau Τ
ι \iota Iota Ι
κ \kappa Kappa Κ
ν \nu Nu Ν
ο \omicron Omicron Ο
ς \sigmaf Upsilon Υ
ς \varsigma
ϑ \vartheta
υ \upsilon
φ \phi Phi Φ
χ \chi Chi Χ
ψ \psi Psi Ψ
ω \omega Omega Ω
ϖ \varpi
ϱ \varrho

### 其他特殊字符

任意的∀：\forall　　
无穷大∞：\infty　　
空集∅：\emptyset　　
存在∃：\exists　　
梯度∇：\nabla　　
垂直⊥：\bot　　
角∠：\angle　　
小于等于≤：\le　　
大于等于≥：\ge　　
恒等于≡：\equiv　　
因为∵：\because　　
所以∴：\therefore　　
乘号×：\times　　
向量点击符号⋅：\cdot　　
角度的显示30∘：30^circ 或 30∘：30^@

### 极限与积分，累加与累乘

#### 极限

若没有底标，则为\lim{正体}
例如 : \lim{(a+b)}

如果有底标，则为\lim_{底标}{正体}
例如 : \lim_{x->0}{f(x)}

#### 积分

(1) \int{正体}

(2) \int_{底标}^{顶标}{正体}
例如 : \int_{i=0}^{5}{a_i}

#### 累加与累乘

累加为 \sum 累乘为 \prod

(1)没有底标时，写作 \sum{}
例如 : \sum{a}

(2)有底标或顶标时，写作 \sum_{}^{}{}

其中_{}和^{}写和不写都可以，但是如果都出现的话，记得先底标再顶标

例如 : \sum_{n=1}^{4}{a_n}

#### 其他常用

1. 上标与下标

上标命令是 ^{角标}，下标命令是 _{角标}。当角标是单个字符时可以不用花括号

1. 分式

输入较短的分式时，最简单的方法是使用斜线，譬如输入 (x+y)/2，可得到(x+y)/2 。

要输入带有水平分数线的公式，可用命令：\frac{分子}{分母}。

1. 根式

排版根式的命令是：开平方：\sqrt{表达式}；开 n 次方：\sqrt[n]{表达式}

1. 矩阵

对于少于 10 列的矩阵，可使用 matrix，pmatrix，bmatrix，Bmatrix，vmatrix 和 Vmatrix

$$\begin{matrix}1 & 2\3 &4\end{matrix}$$
$$\begin{pmatrix}1 & 2\3 &4\end{pmatrix}$$
$$\begin{bmatrix}1 & 2\3 &4\end{bmatrix}$$
$$\begin{Bmatrix}1 & 2\3 &4\end{Bmatrix}$$
$$\begin{vmatrix}1 & 2\3 &4\end{vmatrix}$$
$$\begin{Vmatrix}1 & 2\3 &4\end{Vmatrix}$$