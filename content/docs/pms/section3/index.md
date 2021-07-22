---
title: "随机向量及其分布"
description: ""
lead: ""
date: 2021-04-10T11:01:48+08:00
lastmod: 2021-04-10T11:01:48+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pms"
weight: 33
toc: true
---

## 1. 二维随机变量及其分布函数

### 一. 二维离散型随机向量

+ 定义: 若二维随机向量$(X,Y)$的每个分量都是离散型的随机变量，则称$(X,Y)$为一个二维离散型随机向量.

+ 定义: 设随机向量$(X,Y)$的虽有可能取值为$(x_i,y_j)， i,j = 1,2...$,假设当$i\neq k , j \neq l$时$x_i\neq x_k,y_j\neq y_l$,则$P(X = x_i, Y = y_j)  = p{ij},i,j  = 1,2...$称为随机向量$(X,Y)$的联合分布率或者$X$和$Y$的联合概率分布。

+ 定义: 对任意的实数$x,y$，称二元函数$F(x,y) = P(X\leq x, Y\leq y)$为随机变量$(X,Y)$的联合分布函数，记为$(X,Y)\sim F(x,y)$

+ 分布函数的性质:

  - 单调不减性;

  - $0\leq F(x,y)\leq 1$，且任意固定的$y\in R$,$F(-\infty,y) = \lim\limits_{x->-\infty}F(x,y) = 0$，任意固定的$x\in R$同理。且$F(-\infty,-\infty) = 0$， $F(+\infty, +\infty) = 1$

  - $F(x,y)$对$x$和$y$是有连续的,即
    $$
    F(x,y) = \lim\limits_{z->x^+}F(z,y)\newline
    F(x,y) = \lim\limits_{z->y^+}F(x,z)
    $$

  - 对于任意实数$x_1\leq x_2, y_1\leq y_2$
    $$
    F(x_2,y_2) - F(x_1,y_2)-F(x_2,y_1)+F(x_1,y_1)\geq 0
    $$

+ 任何二维随机变量的分布函数都具有上述四条性质

+ 边缘分布函数:

  - $F_X(x) = P(X\leq x) = P(X\leq x, Y < +\infty) = \lim\limits_{y\rightarrow+\infty}P(X\leq x, Y\leq y) = F(x,+\infty)$
  - $F_Y(y) = F(+\infty,y)$

### 二. 二维连续型随机向量

+ 设二维随机向量$(X,Y)$的分布函数为$F(x,y)$,若存在非负可积二元函数$p(x,y)$,对任意的$x,y\in R$,有
  $$
  F(x,y) = \int_{-\infty}^x\int_{-\infty}^yp(u,v)dudv
  $$
  则称$(X,Y)$为二维连续型随机向量,而称$P(x,y)$为$(X,Y)$的一个联合概率密度函数,简称联合密度.

+ 边缘密度函数

  - $p_X(x) = \int_{-\infty}^{+\infty} p(x,v)dv$
  - $p_Y(y) = \int_{-\infty}^{+\infty}p(u,y)du$

+ 若二维随机向量$(X,Y)$的联合分布函数为,任意$x,y\in R$,
  $$
  p(x,y) = \frac{1}{2\pi\sigma_1\sigma_2\sqrt{1-\rho^2}}exp\{-\frac{1}{2(1-\rho^2)}[(\frac{x-\mu_1}{\sigma_1})^2-2\rho(\frac{x-\mu_1}{\sigma_1})(\frac{x-\mu_2}{\sigma_2}) + (\frac{y-\mu_2}{\sigma_2})^2]\}
  $$
  其中,$\mu_1,\mu_2\in R$,$\sigma_1,\sigma_2>0,|\rho|<1$均为常数,则称$(X,Y)$服从二元正态分布,记为$(X,Y)\sim N(\mu_1,\mu_2,\sigma_1,\sigma_2,\rho)$.

  其中$X$的边缘密度函数$X\sim N(\mu_1,\sigma_1^2)$,$Y$的边缘密度函数$Y\sim N(\mu_2,\sigma_2^2)$.

## 2. 条件分布

### 一. 离散型随机向量的条件概率分布

+ $$
  P(Y=y_j|X=x_i) = \frac{P(X=x_i,Y=y_j)}{P(X=x_i)} = \frac{p_{ij}}{p_i}
  $$

### 二. 连续型随机向量的条件概率

+ 在条件$X=x$的条件下,$Y$的条件分布函数
  $$
  F_{Y|X = x}(y) = P(Y\leq y|X=x)=\int_{-\infty}^y\frac{p(x,v)}{p_X(x)}dv
  $$

+ 密度函数
  $$
  p_{Y|X=x}(y) = \frac{p(x,y)}{p_X(x)}
  $$

## 3. 随机变量的独立性

+ 设$X$和$Y$是两个离散型随机变量,若任意一组$(x_i,y_j)$,
  $$
  P(X=x_i, Y=y_j) = P(X=x_i)P(Y=y_j)
  $$
  即对于任意的$i,j=1,2...,p_{ij} = p_i\cdot p_j$,则称随机变量$X$和$Y$相互独立.

+ 设$(X,Y)$有联合密度函数$p(x,y)$,则$X$和$Y$独立的充要条件是,存在函数$g_1(x)$和$g_2(y)$,使得
  $$
  p(x,y) = g_1(x)g_2(y)
  $$

## 4. 二维随机向量函数的分布

### 一. 二维离散型随机向量函数的分布

+ 随机变量和的分布:

  设$Z = X+Y$,其分布率为
  $$
  P(Z = z_k) = \sum\limits_{x_i}P(X=x_i,Y=z_k-x_i)
  $$

+ 

### 二. 二维连续型随机向量函数的分布

+ 和的分布:

  设$(X,Y)$有联合密度函数$p(x,y)$,$Z=X+Y$,则$Z$是一个连续型随机变量,其密度函数为
  $$
  p_Z(z) = \int_{-\infty}^{+\infty}p(x,z-x)dx
  $$
  特别,当$X$和$Y$独立,且$X$有密度函数$p_X(x)$,$Y$有密度函数$p_Y(y)$时,由于$p(x,y) = p_X(x)p_Y(y)$,故而$Z$的密度函数为
  $$
  p_Z(z) = \int_{-\infty}^{+\infty}p_X(x)p_Y(z-x)dx
  $$
  这个等式称为$p_X(x)$和$p_Y(y)$的卷积公式.

+ 商的分布:

   $(X,Y)$有联合密度函数$p(x,y)$,$Y\neq 0$,定义$Z=X/Y$,则$Z$是一个连续型随机变量,其密度函数为:
  $$
  p_Z(z) = \int_{-\infty}^{+\infty}|y|p(zy,y)dy
  $$

+ 最大(小)值的分布

  设随机变量$X$和$Y$相互独立,且分别有分布函数$F_X(x)$和$F_Y(y)$,定义$M=\max(X,Y)$,$N=\min(X,Y)$,则$M$的分布函数为$F_M(z) = F_X(z)F_Y(z)$,$N$的分布函数为$F_N(z) = 1-(1-F_X(z))(1-F_Y(z))$

+ 两独立的指数分布随机变量的最小值也服从指数分布,且其参数是两个参数的和.
+ 若$X$和$Y$独立,且$X\sim N(\mu_1,\sigma_1^2)$,$Y\sim N(\mu_2,\sigma_2^2)$,则$X+Y\sim N(\mu_1+\mu_2,\sigma_1^2+\sigma_2^2)$