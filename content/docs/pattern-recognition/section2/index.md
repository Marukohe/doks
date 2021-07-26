---
title: "2 - 概率、统计、线性代数极简回顾"
description: ""
lead: ""
date: 2021-04-10T09:54:11+08:00
lastmod: 2021-04-10T09:54:11+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 23
toc: true
---

## 线性代数

### 向量

+ $\bm{x} = (x_1,x_2,...,x_d)^\top\in \mathbb{R}^d$
+ 内积（点积）
  $\bm{x^\top}\bm{y}=\bm{y^\top}\bm{x}=\sum_i^dx_iy_i$
+ 向量的长度（verctor）
  + $\Vert\bm{x}\Vert=\sqrt{x^\top x}, \Vert\bm{x}\Vert^2=\bm{x}^\top\bm{x}$
  + 若$\Vert\bm{x}\Vert = 1$，称$\bm{x}$为单位向量
+ 正交
  + $\bm{x}^\top\bm{y} = 0$
  + $\bm{x}$和$\bm{y}$被称为垂直：$\bm{x}\perp\bm{y}$

### 内积、角度、投影

+ $x$：$\Vert x\Vert$决定长度，$\frac{x}{\Vert x\Vert}$决定方向
+ 向量之间的夹角
  + $\bm{x}^\top\bm{y}=\Vert \bm{x}\Vert\cdot\Vert \bm{y}\Vert\cdot \cos\theta$
  + $\Vert\bm{x}^\top\bm{y}\Vert\leq \Vert\bm{x}\Vert\cdot\Vert\bm{y}\Vert$
+ $\bm{x}$在$\bm{y}$上的投影
  + 方向：$\frac{\bm{y}}{\Vert\bm{y}\Vert}\quad$
  + 长度：$\Vert\bm{x}\Vert\cos\theta = \Vert\bm{x}\Vert\frac{\bm{x}^\top\bm{y}}{\Vert\bm{x}\Vert\cdot\Vert\bm{y}\Vert}=\frac{\bm{x}^\top\bm{y}}{\Vert\bm{y}\Vert}$
  + 投影$\text{proj}_{\bm{y}}\bm{x}: \frac{\bm{x}^\top\bm{y}}{\Vert\bm{y}\Vert^2}\bm{y}$
  + $\text{proj}_{\bm{y}}\bm{x}\perp \bm{z}$
  + $\text{proj}_{\bm{y}}\bm{x}+\bm{z}=\bm{x}$
  {{< img src="2-1.png" alt="Rectangle" class="border-0" >}}

### 柯西-施瓦茨不等式

+ Cauchy's inequality
  $$
  (\sum_{k=1}^na_kb_k)^2\leq (\sum_{k=1}^n a_k^2)(\sum_{k=1}^nb_k^2)
  $$
  等号成立`当且仅当`存在固定实数$c$，使得$\forall k,a_k=cb_k$

+ Schwarz's Inequality
  $$
  \left[\int_a^bf(x)g(x)dx\right]^2\leq\left[\int_a^b[f(x)]^2dx\right]\left[\int_a^b[g(x)]^2dx\right]
  $$
  等号成立`当且仅当`存在固定实数$c$，使得$\forall x\in [a,b], f(x)=cg(x)$（不太准确）

### 矩阵

+ $m\times n$的矩阵
  $$
  \mathbf{X}=\begin{bmatrix}
  x_{11}&\cdots &x_{1n}\cr
  \vdots&\ddots&\vdots\cr
  x_{m1}&\cdots&x_{mn}
  \end{bmatrix}
  $$
  + $n=m$时称为方阵
  + 行矩阵（行向量）：$m=1$
  + 列矩阵（列向量，向量）：$n=1$
+ 对角阵：方阵中，只有对角线非零
+ 单位阵：对角线全部为1的对角阵
  + 一般记作$I$或$I_n$

### 矩阵运算

+ 乘法：$\bm{X}：m\times n,\bm{Y}： n\times p$
  + 维度相符时乘法才有定义
  + 一般来说$\bm{X}\bm{Y}\neq\bm{Y}\bm{X}$
+ 矩阵的幂
  + 对方阵有定义：$\bm{X}^2=\bm{X}\bm{X},\bm{X}^2=\bm{X}\bm{X}\bm{X},\cdots$
+ 转置
  + $\bm{X}:m\times n$，那么$\bm{X}^\top:n\times m$
  + $\bm{X}^\top\bm{X}:n\times n,\bm{X}\bm{X}^\top:m\times m$
+ 对称矩阵
  + 是方阵，$\bm{X}_{ij}=\bm{X}_{ji},\forall i,j$

### 行列式值、矩阵的逆

+ 方阵的行列式值
  + $|\bm{X}|$，或写作$\det(X)$
  + $|\bm{X}|=|\bm{X}^\top|$
  + $|\bm{X}\bm{Y}|=|\bm{X}||\bm{Y}|$
  + $|\lambda\bm{X}|=\lambda^n|\bm{X}|\quad (\bm{X}:n\times n)$
  
+ 方阵的逆矩阵
  + $\bm{X}^{-1}$：满足$\bm{X}\bm{X}^{-1}=\bm{X}^{-1}\bm{X}=I_n$
  + $\bm{X}$可逆等价于$|\bm{X}|\neq 0$
  + $(\lambda\bm{X})^{-1}=\frac{1}{\lambda}\bm{X}^{-1}$
  + $(\bm{X}\bm{Y})^{-1}=\bm{Y}^{-1}\bm{X}^{-1},(\bm{X}^{-1})^\top=(\bm{X}^\top)^{-1}$

### 方阵的特征值、特征向量、迹

+ 特征值(eigenvalue)和特征向量(eigenvector)
  + $A\bm{x}=\lambda\bm{x}\quad$ $A:n\times n$
  + $\lambda$：特征值$\quad$ $x$：特征向量
+ $n$阶方阵有$n$个特征值
  + 可能存在相等的特征值
+ 特征值和对角线的关系
  + $\sum_{i=1}^na_{ii}=\sum_{i=1}^n\lambda_i$
  + $\det(A)=\prod_{i=1}^n\lambda_i$
+ 方阵的迹
  $\text{tr}(A)=\sum_{i=1}^n a_{ii},\text{tr}(AB)=\text{tr}(BA)$

### 实对称矩阵（Real symmetric matrix）

+ 对称矩阵，每个项都是实数
+ 性质：
  + 所有特征值都是实数，特征向量都是实向量
  + 特征值记为$\lambda_1\geq \lambda_2\geq\cdots\geq\lambda_n$
  + 对应的特征向量记为$\xi_1,\xi_2,\cdots,\xi_n$
  + 特征向量相互垂直：$\xi_i^\top\xi_j=0\quad(i\neq j)$
  + $\bm{E}=[\xi_1\xi_2\cdots\xi_n]$是$n\times n$的，是`满秩`的。

### 实对称矩阵的分解

+ $\bm{X}:n\times n$的实对称矩阵
  + 特征值为$\lambda_i$，其对应的特征向量为$\xi_i$
+ $\bm{X}=\sum_{i=1}^n\lambda_i\xi_i\xi_i^\top$
  + 称为谱分解
  + 约定$\Vert\xi_i\Vert=1$，则$\bm{E}$是正交矩阵
  + $\bm{X}=\bm{E}\Lambda\bm{E}^\top$
    + $\Lambda$是一个对角矩阵，$\Lambda_{ii}=\lambda_i$
    + $\bm{E}\bm{E}^\top=\bm{E}^\top\bm{E}=\bm{I},\bm{E}^{-1}=\bm{E}^\top,|\bm{E}|=1$

### 正定、半正定

+ 对称方阵$A$是正定的当且仅当
  + $\forall\bm{x}\neq 0\quad \bm{x}^\top A\bm{x}=\sum_{i,j}x_ix_jA_{ij}>0$
  + $\forall\bm{x}\neq 0\quad \bm{x}^\top A\bm{x}\geq0$则$A$为半正定
  + 分别记为$A\succ 0$或$A\succcurlyeq 0$
+ $\bm{x}^\top A\bm{x}$：称为二次型
+ 等价关系
  + $A\succ 0(A\succcurlyeq0)$
  + 特征值全部为正数（非负实数）
+ 正定矩阵的任意主子矩阵也是正定矩阵

### 矩阵求导

+ 假设一切求导的条件都满足

+ $\frac{\partial \bm{a}}{\partial x}$是一个向量，$(\frac{\partial\bm{a}}{\partial x})_i=\frac{\partial a_i}{\partial x}$

+ 对于矩阵，  
  $$
  \left(\frac{\partial A}{\partial x}\right)_{ij} = \frac{\partial A_{ij}}{\partial x}
  $$

+ 还有
  $$
  \left(\frac{\partial x}{\partial \bm{a}}\right)_i = \frac{\partial x}{\partial a_i}
  $$

  $$
  \left(\frac{\partial x}{\partial A}\right)_{ij}=\frac{\partial x}{\partial A_{ij}}
  $$

  $$
  \left(\frac{\partial \bm{a}}{\partial x}\right)_{ij}=\frac{\partial a_i}{\partial x_j}
  $$

## 概率与统计

### 随机变量

+ $\bm{X}$：可以是离散、连续或者混合的

### 概率密度函数

+ （古典）离散
  + 可数的不相容的若干事件$x_1,x_2,\cdots$
  + $p(X=x_i)=c_i$
  + $c_i\geq 0,\sum_ic_i=1$
+ 连续：为简化，只考虑$X\in(-\infty,+\infty)$
  + $p(x)$：概率密度函数(pdf)
  + $p(x)\geq 0;\int_{-\infty}^{+\infty}p(x)dx = 1$

### 分布函数（连续）(cdf)

+ $F(x)=P(X\leq x) = \int_{-\infty}^xp(x)dx$
  + $F(-\infty) = 0\leq F(x)\leq F(+\infty) = 1$
  + 非减性，如果$x\leq y$，那么$F(x)\leq F(y)$
  + $P(x_1<x<x_2)=F(x_2)-F(x_1)=\int_{x_1}^{x_2}p(x)dx$
+ PDF和CDF的关系
  + $p(x)=F^\prime(x)$

### 联合、条件分布、变换

+ 联合：$P(X=x)$
  + $p(x)\geq 0\quad \int p(x)dx=1$

+ 条件：$P(X=x\mid Y=y)$

+ $p(x,y)=p(y)p(x\mid y)$

+ $p(x)=\int_y p(x,y)dy$

+ 假设$x=g(y)$，那么
  $$
  p_Y(y)=p_X(x)\left|\frac{dx}{dy}\right|=p_X(g(y))\left|g^\prime(y)\right|
  $$

### 多维分布的期望

+ 假设有函数$f(x)$，在$x$服从分布$p(x)$时：
+ $f$的期望，记为$E[f(X)]$
  + $E[f(X)]=\sum_xf(x)\cdot p(X=x)$或
  + $E[f(X)]=\int f(x)p(x)dx$
  + 条件期望$E(f(x)\mid Y=y)=\sum_xf(x)\cdot p(x\mid y)$
+ $f$的方差（一维）或协方差（多维）
  + $Var(X)=E[(X-EX)]^2$
  + $Var(X)=E(X^2)-(EX)^2$
+ 当$p(x)$和$f(x)$固定时
  + 期望、方差是一个确定的数

### 估计均值和协方差矩阵

+ 训练样本：$x_1,x_2,\cdots,x_n$
+ 均值的估计：
  $$
  \bar{x}=\frac{1}{n}\sum_{i=1}^nx_i
  $$
+ 协方差的估计
  $$
  Cov(x)=\frac{1}{n}\sum_{i=1}^n(x_i-\bar{x})(x_i-\bar{x})^\top
  $$
  无偏估计
  $$
  Cov(x)=\frac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})(x_i-\bar{x})^\top
  $$

### 两个随机变量的独立、相关

+ 如果$\forall x, y$，$p(x,y)=p(x)p(y)$，则$X,Y$互相独立
+ $cov(X,Y)=E[(X-EX)(Y-EY)]$
+ Pearson相关系数：$-1\leq \rho_{XY}\leq 1$
  $$
  \rho_{XY}=\frac{cov(X,Y)}{\sqrt{Var(X)Var(Y)}}
  $$  
  + $\rho_{XY}=0$，称为不相关
  + $\rho_{XY}=\plusmn 1$，称为完全相关，如存在相关系数
  + 独立保证一定相关，但是，不相关不一定能保证独立

### 高斯分布

+ 又叫正态分布
+ 单变量或一维高斯分布$N(\mu,\sigma^2)$
  $$
  p(x)=(2\pi)^{-\frac{1}{2}}(\sigma^2)^{-\frac{1}{2}}\exp\\{-\frac{1}{2}(x-\mu)(\sigma^2)^{-1}(x-\mu)\\}
  $$
+ Markov不等式：若$X\geq 0$，则$P(X\geq a)\leq\frac{EX}{a}$
+ Chebyshev不等式：对任何分布，$P((X-\mu)^2\geq k^2)\leq\frac{\sigma^2}{k^2}$或者$P(|X-\mu|>k)\leq\frac{\sigma^2}{k^2}$

### 多维高斯分布

+ 多维
  $$
  p(x)=(2\pi)^{-\frac{D}{2}}|\Sigma|^{-\frac{1}{2}}\exp\\{-\frac{1}{2}(x-\mu)^\top\Sigma^{-1}(x-\mu)\\}
  $$
+ D：维数
+ $\Sigma$：协方差矩阵
+ $\mu$：均值

### 高斯分布中的相关性和独立

+ 一般来说，两变量
  + 独立保证一定不相关
  + 不相关不一定保证独立
+ 但是，对于多维高斯分布
  + 不相关意味着协方差矩阵中非对角线项是0
  + 在正态分布中，不相关就等价于独立

### 多维与一维高斯的关系

+ 多维高斯
  $$
  X=\left(\begin{matrix}
    X_a\cr
    X_b
  \end{matrix}\right)
  $$
  + 条件分布：$x_a\mid x_b$还是高斯分布
  + 边际分布：$p(x_a)=\int p(x_a,x_b)dx_b$也是高斯分布
