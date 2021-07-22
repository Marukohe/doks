---
title: "4 - 主成分分析"
description: ""
lead: ""
date: 2021-04-10T09:54:17+08:00
lastmod: 2021-04-10T09:54:17+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 25
toc: true
---

### 与领域无关的特征提取
+ 第4讲：PCA
+ 第5讲：特征的归一化
+ 第5讲：FLD

## PCA基础

### 常见的数据特点 
+ 数据各唯独之间不是互相独立的
  + 数据的内在维度通常远低于其表面维度
  + 因此，需要降低数据维度
  + PCA在降维方法中是最常用的一种
+ 线性降维方法：找到向量$w_i$与原向量内积得到实数值，多个值组成一个降维后的向量

### Starting point：零阶表示
+ Zero-dimensional representation
+ 不允许使用任何维度，如何最佳表示$\boldsymbol{x}$
+ 寻找某个固定的$\boldsymbol{m}$，使得
  $$
  J_1(\boldsymbol{m})=\min_\boldsymbol{m}\sum_{i=1}^n\\|x_i-m\\|^2
  $$
+ 最优解：（证明？）
  $$
  \boldsymbol{m}^*=\argmin_{\boldsymbol{m}}\sum_{i=1}^n\\|x_i-m\\|^2=\frac{1}{n}\sum_{i=1}^n x_i
  $$

### 一维表示：数据维度间的线性关系
+ 数据是$d$维，但是内在维度可能$m$维的，$m<d$或者$m<<d$，PCA用线性关系来降低维度
+ $\boldsymbol{x}\in\mathbb{R}^d$：原来的高维数据（随机变量）
  + 训练样本：$\boldsymbol{x}_1,\boldsymbol{x}_2,...,\boldsymbol{x}_n$
+ 假设$m=1$，用原数据的单个线性组合来表示
  + $y_i=\boldsymbol{w^\top x}_i+b$
  + $y_1,...,y_n$ ——新的数据/特征

### 形式化formalization：最大化方差
+ 方差是衡量新特征包含信息多少的度量
  + 有时也称为能量energy
+ 优化目标函数$J_2(\boldsymbol{w})=\frac{1}{n}\sum_{i=1}^n\\|\boldsymbol{w}^\top(\boldsymbol{x_i}-\boldsymbol{\bar{x}})\\|^2$
+ 发现问题：
  + $J_2(\boldsymbol{w})$可以是无穷大或者无穷小
  + 最常用的解决方法：加上限制条件$\\|\boldsymbol{w}\\|^2=\boldsymbol{w}^\top\boldsymbol{w}=1$
    $$
    \argmax_{\boldsymbol{w}}\frac{1}{n}\sum_{i=1}^n\\|(\boldsymbol{x}_i-\boldsymbol{\bar{x}})^\top\boldsymbol{w}\\|^2\newline
    s.t.\quad \boldsymbol{w}^\top\boldsymbol{w}=1
    $$
  + s.t. ——subject to，表示约束条件constraints

### 简化simplification 变换transformation
+ 
  $$
  \\|(\boldsymbol{x}_i-\boldsymbol{\bar{x}})^\top\boldsymbol{w}\\|^2=((\boldsymbol{x}_i-\bar{x})^\top\boldsymbol{w})^\top((\boldsymbol{x}_i-\bar{x})^\top\boldsymbol{w})=\boldsymbol{w}^\top(\boldsymbol{x}_i-\boldsymbol{\bar{x}})(\boldsymbol{x}_i-\boldsymbol{\bar{x}})^\top\boldsymbol{w}
  $$
+ 
  $$
  \frac{1}{n}\sum_{i=1}^n\\|(\boldsymbol{x}_i-\boldsymbol{\bar{x}})^\top\boldsymbol{w}\\|^2 = \boldsymbol{w}^\top\sum_{i=1}^n(\boldsymbol{x}_i-\boldsymbol{\bar{x}})(\boldsymbol{x}_i-\boldsymbol{\bar{x}})^\top\boldsymbol{w}=\boldsymbol{w}^\top Cov(\boldsymbol{x})\boldsymbol{w}
  $$

### 优化optimization
+ 拉格朗日乘子法
  + 将有约束的优化问题转化为无约束的优化问题
+ Lagrangian 拉格朗日函数
  $$
  f(\boldsymbol{w},\lambda) = \boldsymbol{w}^\top Cov(\boldsymbol{x})\boldsymbol{w} - \lambda(\boldsymbol{w}^\top\boldsymbol{w} - 1)
  $$
+ $\lambda$：拉格朗日乘子
+ 最优的必要条件：$\frac{\partial f}{\partial\boldsymbol{w}} = 0$，$\frac{\partial f}{\partial\lambda} = 0$
+ $\frac{\partial f}{\partial\boldsymbol{w}} = 2Cov(\boldsymbol{x})\boldsymbol{w}-2\lambda\boldsymbol{w}=0$
+ $Cov(\boldsymbol{x})\boldsymbol{w} = \lambda\boldsymbol{w}$，$\boldsymbol{w}^\top\boldsymbol{w}=1$
+ 简化为特征分解问题

### 选哪个特征向量
+ $J_2\boldsymbol{w}=\boldsymbol{w}^\top Cov(\boldsymbol{x})\boldsymbol{w}=?$
+ $Cov(\boldsymbol{x})$是半正定的
+ 选取$\lambda_1$（即最大特征值）对应的特征向量$\boldsymbol{\xi}_1$为$\boldsymbol{w}_1$
+ 怎样用$\boldsymbol{w}_1$来近似$\boldsymbol{x}$
  + 投影！
  + $\boldsymbol{x}\approx\boldsymbol{\bar{x}}+(\boldsymbol{w_1}^\top(\boldsymbol{x}-\boldsymbol{\bar{x}}))\boldsymbol{w}_1$
  + 所以$\boldsymbol{y}_i=\boldsymbol{w_1}^\top(\boldsymbol{x}-\boldsymbol{\bar{x}})$
  + 那么，$b=\boldsymbol{w_1}^\top\boldsymbol{\bar{x}}$

### $J_1$和$J_2$的等价关系
+ 若干向量
  + $\boldsymbol{x}_i$：降维之前的向量
  + $\boldsymbol{w}_1^\top(\boldsymbol{x}_i-\boldsymbol{\bar{x}})\boldsymbol{w}_1=y_i\boldsymbol{w}_1$：降维之后的向量
  + $\hat{\boldsymbol{x}}$：在原空间中重建的向量
  + 目前的重建关系：$\hat{\boldsymbol{x}}_i\approx \boldsymbol{\bar{x}}+y_i\boldsymbol{w}_1$
+ $J_1$的目的是使得$\hat{\boldsymbol{x}}_i$和$\boldsymbol{x}_i$尽可能相差小($\boldsymbol{\bar{x}}$固定为均值)
  + $J_1(\boldsymbol{w,a})=\sum_{i=1}^n\frac{1}{n}\\|\boldsymbol{x}_i-(\boldsymbol{\bar{x}}-a_i\boldsymbol{w})\\|^2$
  + $\boldsymbol{w}$：投影方向，$a_i$：投影系数
+ 最小化$J_1$得到的$a_i$和$\boldsymbol{w}$与$J_2$得到的结果完全一致

### 如果需要更多投影方向？
+ What if we need $\boldsymbol{w}_2,\boldsymbol{w}_3$
  + 新的投影方向需要继续保持“能量”
  + 但是需要限制
    $$
    \boldsymbol{w}_2\perp \boldsymbol{w}_1,\boldsymbol{w}_3\perp\boldsymbol{w}_2...
    $$
+ 在上述限制条件下
  $$
  \boldsymbol{w}_2=\boldsymbol{\xi}_2,\boldsymbol{w}_3=\boldsymbol{\xi}_3
  $$
  重建系数：$\boldsymbol{w}_j^\top(\boldsymbol{x}-\boldsymbol{\bar{x}})$
+ 总之，
  $$
  \boldsymbol{x}\approx \boldsymbol{\bar{x}}+(\boldsymbol{w}^\top(\boldsymbol{x}-\boldsymbol{\bar{x}}))\boldsymbol{w}_1+...
  $$

### 重建和原数据的关系
+ 假设$n>d$，即数据比维数多
  + 进一步假设$Cov(\boldsymbol{x})$可逆
  + 如果$n<d$，那么情况如何，因为最大的特征值还是大于0的，仍然能做PAC变换
+ $Cov(\boldsymbol{x})$是$d\times d$矩阵，有$d$个互相垂直的特征向量$\boldsymbol{\xi}_i$
  + 重建会有$d$个互相垂直的投影方向$\boldsymbol{w}_i$
    $$
    \forall\boldsymbol{x}, \quad \boldsymbol{x} = \boldsymbol{\bar{x}}+\sum_{i=1}^d(\boldsymbol{w}_i^\top(\boldsymbol{x}-\boldsymbol{\bar{x}}))\boldsymbol{w}_i
    $$
  + 将$\boldsymbol{w}_i$拼接成矩阵形式$W=[\boldsymbol{w}_1,...,\boldsymbol{w}_d](d\times d)$
  + 投影系数是$W^\top(\boldsymbol{x}-\boldsymbol{\bar{x}})$，投影方向是$W$
  + $\boldsymbol{x}=\boldsymbol{\bar{x}}+WW^\top(\boldsymbol{x}-\boldsymbol{\bar{x}})$
  + 重建是完全精确的（没有误差）

### 降维
+ 很多时候，有些投影方向是噪声
  + 需要扔掉一些方向
+ 去掉特征值最小的那些
+ 通常保持90%的能量
  $$
  \frac{\lambda_1+\lambda_2+...+\lambda_T}{\lambda_1+\lambda_2+...+\lambda_d}>0.9
  $$
  寻找第一个$T$，使得上面的不等式成立

### 降维的损失
+ 现在$\hat{W}=[\boldsymbol{w}_1...\boldsymbol{w}_T](d\times T)$
+ $\boldsymbol{x}-\boldsymbol{\hat{x}}=\sum_{j=T+1}^d(\boldsymbol{w}_j^\top(\boldsymbol{x}-\boldsymbol{\bar{x}}))\boldsymbol{w}_j=\sum_{j=T+1}^d\boldsymbol{e}_j$
  + 这个误差多大
  + $\boldsymbol{e}_j^\top\boldsymbol{e}_k=0$，如果$j\neq k$
  + $E(\\|\boldsymbol{x}-\boldsymbol{\hat{x}}\\|^2)=\sum_{j=T+1}^dE(\\|\boldsymbol{e}_j\\|^2)$
  + $E(\\|\boldsymbol{e}_j\\|^2)=\lambda_j$
+ 这样降维保证平均（期望）重建误差最小
  + 直接优化重建误差$J_1$得到同样的结果

### 小结：PCA变换的步骤
+ 训练样本：$\boldsymbol{x}_1,\boldsymbol{x}_1,...,\boldsymbol{x}_n$
+ 计算得到$\boldsymbol{\bar{x}}$和$Cov(\boldsymbol{x})$
+ 求得$Cov(\boldsymbol{x})$的特征值和特征向量
+ 根据特征值选定$T$
+ 根据$T$的值确定矩阵$\hat{W}$
+ 对任何数据$\boldsymbol{x}$，其新的经过PCA变换得到的特征是
  $$
  \boldsymbol{y}=\hat{W}^\top(\boldsymbol{x}-\boldsymbol{\bar{x}})
  $$
  重建则为$\boldsymbol{x}\approx\boldsymbol{\hat{x}}=\boldsymbol{\bar{x}}+\hat{W}\boldsymbol{y}$

## 正态分布与PCA
### PCA vs. Gaussian
+ $\boldsymbol{x}$服从$D$维高斯分布$N(\boldsymbol{\mu},\Sigma)$
  $$
  p(\boldsymbol{x})=(2\pi)^{-\frac{D}{2}}|\Sigma|^{-\frac{1}{2}}\exp\\{-\frac{1}{2}(\boldsymbol{x-\boldsymbol{\mu}})^\top\Sigma^{-1}(\boldsymbol{x}-\boldsymbol{\mu})\\}
  $$

### PCA of Gaussian
+ $\boldsymbol{x}\sim N(\boldsymbol{\mu},\Sigma)$
+ 假设使用全部特征向量，则$\boldsymbol{y}=W^\top(\boldsymbol{x}-\boldsymbol{\mu})$
  + $\Sigma = \sum_{i=1}^d\lambda_i\boldsymbol{\xi}_i\boldsymbol{\xi}_i^\top=\sum_{i=1}^d\lambda_i\boldsymbol{w}_i\boldsymbol{w}_i^\top=W\Lambda W^\top$
    + $\Lambda$是一个对角阵，$\Lambda_{ii}=\lambda_i$
  + $E\boldsymbol{y} = 0$
+ $y\sim N(0,\Lambda)$
+ PCA旋转了数据，使得新特征各个维度互不相关
  + 对高斯分布，不相关意味着独立！

### PCA的有点
+ 减少了数据量
  + 可以减少计算量，缩短训练、测试、识别时间
  + 可以减少所需的存储空间
    + 对大规模数据特别重要
  + 可能去除数据中的噪声
    + 所以可能提高系统的识别精确度
+ 如果数据服从高斯分布
  + 完成PCA后，新特征维度互不相关
  + 有利于模式识别

### 白化变换
+ 协方差矩阵$\Sigma$，可以从训练样本估计
  + PCA：$\boldsymbol{y}=W^\top(\boldsymbol{x}-\boldsymbol{\mu})$
  + $\Sigma = \sum_{i=1}^d\lambda_i\boldsymbol{\xi}_i\boldsymbol{\xi}_i^\top=\sum_{i=1}^d\lambda_i\boldsymbol{w}_i\boldsymbol{w}_i^\top=W\Lambda W^\top$
+ 白花变换
  + $\boldsymbol{y}=(W\Lambda^{-\frac{1}{2}})^\top(\boldsymbol{x}-\boldsymbol{\mu})$
+ $\boldsymbol{y}\sim N(0,I)$

### 高斯假设
+ PCA变换
  + PCA变换不一定要求$\boldsymbol{x}$服从高斯分布
  + $\boldsymbol{x}$不服从高斯分布时，$E(\boldsymbol{y})=\boldsymbol{0},Cov(\boldsymbol{y})=\Lambda$，但$\boldsymbol{y}$不服从高斯分布
  + $\boldsymbol{x}$不服从高斯分布时，$\boldsymbol{y}$的各维度不相关，但不独立
+ 白化变换
  + 白化变换不一定要求$\boldsymbol{x}$服从高斯分布
  + $\boldsymbol{x}$不服从高斯分布时，$E(\boldsymbol{y})=\boldsymbol{0},Cov(\boldsymbol{y})=\boldsymbol{I}$，但$\boldsymbol{y}$不服从高斯分布
  + $\boldsymbol{x}$不服从高斯分布时，$\boldsymbol{y}$的各维度不相关，但不独立

### Can I use PCA?
+ 如果数据服从高斯分布
  + 单峰分布
  + 白噪声
    + $\boldsymbol{x}=\boldsymbol{x}'+\boldsymbol{\epsilon}$
    + $\boldsymbol{\epsilon}\sim N(0,\Gamma)$
    + 噪声独立于数据，噪声均值为$\boldsymbol{0}$，噪声各维互相独立($\boldsymbol{\Gamma}$是对角阵)，噪声幅度有限
    + 此时PCA效果最佳
+ 实际上，如果特征值服从指数递减即可
+ PCA在处理离群值的时候效果较差