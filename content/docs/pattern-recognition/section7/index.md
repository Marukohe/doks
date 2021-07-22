---
title: "7 - 推理和决策 密度估计"
description: ""
lead: ""
date: 2021-04-10T09:54:25+08:00
lastmod: 2021-04-10T09:54:25+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 28
toc: true
---

# 推理和决策
## 从统计学statistics的观点看
+ 目的是得到映射：$\mathcal{X}\mapsto\mathcal{Y}$
  + 数据分布$p(\mathcal{X})$
  + 先验分布$p(\mathcal{Y})$
  + 联合分布$p(\mathcal{X},\mathcal{Y})$
  + 类条件分布$p(\mathcal{X}\mid y=i)$
  + 后验分布$p(y=i\mid\boldsymbol{x})$

## 如何表示/估计概率密度
+ Parametric(参数化)：假设PDF服从某种函数形式
  + 如高斯分布的函数形式，其包含若干参数
  + 当指定其所有参数值之后，PDF就完全确定
  + 不同的概率分布由不同的参数值决定
  + 估计PDF就是估计参数
  + 所以叫参数估计

## 非参数估计
+ Non-parametric：不假设PDF是任何已知形式的函数
  + 从直观上更合理
  + 那么，如何估计？
    + 使用训练数据直接估计空间中任意点的密度
    + $p(\boldsymbol{x}\mid D)$
+ 非参数不代表无参数
  + 实际上是可以允许无穷多的参数
  + 而参数估计的参数个数是有限的
+ 参数估计和无参数估计
  + parametric ans non-parametric estimate
  + 可能参数化估计与非参数化估计更切合其含义

## 推理与决策
+ 生成模型和判别模型
  + 生成模型：估计$p(\boldsymbol{x}\mid y = i)$和$p(\boldsymbol{y})$
  + 判别模型：直接估计$p(y=i\mid \boldsymbol{x})$
+ 这些模型分为两个步骤：
  + 推理：估计各种密度函数
  + 决策：根据估计得到的PDF对任意的$\boldsymbol{x}$给出输出

# 参数估计
## 以高斯分布为例
+ 假设$x\sim N(\mu, \sigma^2)$，从数据$D=\\{x_1,...,x_n\\}$估计
  + 数据独立同分布
+ 参数记为$\theta$，这里$\theta = (\mu,\sigma)$，如何估计？形式化
+ 一种直觉：如果两个不同的参数$\theta_1,\theta_2$
  + 假设$\theta$是参数的真实值，似然是
    $$
    p(D\mid\theta) = \sum_i p(x_i\mid\theta) = \sum_i\frac{1}{\sqrt{2\pi\sigma}}\exp\\{-\frac{(x-\mu)^2}{2\sigma^2}\\}
    $$
  + 若$p(D\mid\theta_1)>p(D\mid\theta_2)$，该选择哪个？

## 易混淆的表示法
+ 目前$\theta$不是随机变量，所以$p(D\mid\theta)$不是分布
  + $D$固定，$\theta$是变量，$p(D\mid\theta)$是$\theta$的变量，不是一个PDF！
  + $p(x_i\mid D)$是一个PDF，因为$\theta$不是随机变量，这不是一个条件分布，只是习惯上这么写，表明这个分布依赖于参数$\theta$的值，$x_i$是PDF的变量
+ 较好的表示法：定义似然函数likelihood function
  + $\hat{l}(\theta) = p(D\mid\theta)=\prod_ip(x_i\mid\theta)$
+ 为了方便，定义对数似然函数log-likelihood function
  + $l(\theta) = \ln p(D\mid\theta) = \sum_i\ln p(x_i\mid\theta)$

## 最大似然估计
+ Maximum likelihood estimation, MLE
  $$
  \theta^*=\argmax_\theta l(\theta)
  $$
+ 高斯分布的最大似然估计
  + 参数为$(\mu,\Sigma)$，数据为$D=\\{x_1,...,x_n\\}$
  + 练习：通过对$l(\theta)$求导发现最佳的参数值，可以查表
    $$
    \mu^* = \frac{1}{n}\sum_{i=1}^nx_i\newline
    \Sigma^*=\frac{1}{n}\sum_{i=1}^n(x_i-\mu^*)(x_i-\mu^*)^\top
    $$

## 最大后验估计及其他
+ Maxumum a posteriori estimation, MAP
  + $\theta^* = \argmax_\theta \hat{l}(\theta)p(\theta)$
  + 即假设参数$\theta$也是随机变量，存在着先验分布
+ 与MLE的关系
  + 假设我们对$\theta$一无所知，那么应该怎样设定$p(\theta)$
  + noninformative prior时，MLE等价于MAP
    + 若$\theta$是离散的随机变量，离散的均匀分布，$p(\theta)=\frac{1}{N}$
    + 若$\theta$是有限区间$[a,b]$的连续随机变量，$p(\theta)=\frac{1}{b-a}$
    + 若$\theta$是$(-\infty,+\infty)$上的随机变量，？
    + 假设$p(\theta)=const$。称为improper prior

## 参数估计的一些性质
+ 如果只有一个样例，参数估计会怎么样？
+ 样例越多，估计越准
+ 渐进性质，研究$n\rightarrow\infty$时的性质，如
  + 一致性：随样本容量增大收敛到参数真值的估计量
+ 其他性质如
  + 无偏估计：只估计量的期望和被估计量的真值相等

## 贝叶斯参数估计
+ Bayesian parameter estimation
  + MLE：视$\theta$为固定的参数，假设存在一个最佳的参数（或参数的真实值是存在的），目的是找到这个值
  + MAP：视$\theta$为一个随机变量，存在分布$p(\theta)$，将其影响（先验分布）带入，但仍然存在最优的参数
  + 以上均成为点估计
+ 在贝叶斯观点中，$\theta$是一个分布/随机变量，所以估计应该是估计一个分布，但不是一个值（点）
  + $p(\theta\mid D)$：这是贝叶斯参数估计的输出，是一个完整的分布，而不是一个点

## 高斯分布参数的贝叶斯估计
+ 参数$\theta$的先验分布$p(\theta)$，数据$D=\\{x_1,...,x_n\\}$，估计$p(\theta\mid D)$。这里假设单变量，只估计$\mu$，方差$\sigma$已知
  + 第一步：设定$p(\mu)$的参数形式：$p(\mu)=N(\mu_0,\sigma_0^2)$，目前假设参数$\mu_0,\sigma_0^2$已知
  + 第二步，贝叶斯定理和独立性得到
    $$
    p(\mu\mid D)=\frac{p(D\mid \mu)p(\mu)}{\int p(D\mid \mu)p(\mu)d\mu}=\alpha p(D\mid \mu)p(\mu)=\alpha\prod_{i=1}^n p(x_i\mid\mu)p(\mu)
    $$
  + 第三步，应用高斯的性质，进一步得到其解析形式

## 解的形式
$$
p(\mu\mid D)N(\mu_n,\sigma_n^2)
$$
+ 均值为$\mu_n=\frac{\sigma^2}{n\sigma_0^2+\sigma^2}\mu_0+\frac{n\sigma_0^2}{n\sigma_0^2+\sigma^2}\mu_{ML}$
  + 其中$\mu_{ML}$为MLE的估计值，即$\mu_{ML}=\frac{1}{n}\sum_{i=1}^n x_i$
+ 方差为$\sigma_n^2$，其值由如下公式确定：$\frac{1}{\sigma_n^2}=\frac{1}{\sigma_0^2}+\frac{n}{\sigma^2}$
+ 先验和数据的综合

## 贝叶斯的进一步讨论
+ 共轭先验
  + 若$p(x\mid\theta)$，存在先验$p(\theta)$，使得$p(\theta\mid D)$和$p(\theta)$有相同的函数形式，从而简化推导和计算
  + 如高斯分布的共轭先验分布仍然是高斯分布
+ 优缺点
  + 理论上非常完备，数据上很优美
  + 推导困难
  + 在数据较多时，学习效果常不如直接用discriminant function

# 非参数估计：假设
## 非参数估计
+ 常用的参数形式基本都是单模的，不足以描述复杂的数据分布：即应该以训练数据自身来估计分布

## 维数灾难
+ Curse of dimensionality

## Kernel density estimation
+ KDE，注意这里的kernel与上一章中的核含义不完全一致，其要求的条件也不完全一致
+ 距离：Parzen window（一维，使用高斯核）
  $$
  p(x)=\frac{1}{n}\sum_{i=1}^n\frac{1}{(2\pi h^2)^\frac{1}{2}}\exp\\{-\frac{|x-x_i|^2}{2h^2}\\}
  $$

# 决策
## 决策、预测
+ 当inference完成之后，如果给定输入$x$
  + 应该给出什么样的输出？
  + 怎么给出？
+ 点估计：
  + 根据参数得到后验概率$p(y\mid x;\theta)$
  + 根据其给出结果（如分类，如何输出）
+ Bayesian decision
  + 输出，也是一个随机变量，称为预测分布
  + 结果通常根据其期望决定，同时还可以给出方差

## 点估计的例子
+ 在0-1风险时，选择后验概率最大的那个类别$\argmax_i p(y=i\mid x;\theta)$
+ 在discriminant function观点下，可以定义函数
  $$
  g_i(x)=p(y=i\mid x;\theta)=\frac{p(x\mid y = i;\theta)p(y=i)}{p(x;\theta)}
  $$
+ 或者定义为$g_i(x)=p(x\mid y=i;\theta)p(y=i)$，为什么？
+ 或者定义为
  $$
  g_i(x)=\ln(p(x\mid y=i;\theta))+\ln(p(y=i))
  $$