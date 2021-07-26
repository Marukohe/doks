---
title: "5 - 特征归一化、Fisher线性判别分析"
description: ""
lead: ""
date: 2021-04-10T09:54:19+08:00
lastmod: 2021-04-10T09:54:19+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 26
toc: true
---

## 特征归一化

### 1. 每维度归一

+ Per-dimension normalization
  + 虚拟的例子（判别性别）
    + 假设用两个特征：身高和体重
    + 如果1. 身高单位毫米，体重单位吨
    + 如果2. 身高单位公里，体重单位克
  + 很多时候，不同的维度需要统一到同样的取值范围！
+ 训练集：$\boldsymbol{x}_1,...,\boldsymbol{x}_n$, $\boldsymbol{x}_i=(x_{i1},...,x_{id})$
  + 对每一维$j$，其数据为$x_{1j},x_{2j},...,x_{nj}$
  + 取其最小值$x_{min,j}$和最大值$x_{max,j}$
  + 对这一维的任何数据
    $$
    x_{ij}\leftarrow\frac{x_{ij}-x_{min,j}}{x_{max,j}-x_{min,j}}
    $$

### 稀疏数据

+ 新数据范围是？各维度统一了吗？
  + [0,1]
  + 若某一维$x_{max,j}=x_{min,j}$：这一维是常数，对模式识别并没有作用，可以直接删除。
  + 也可以统一到[-1,1]
    $$
    x_{ij}\leftarrow 2\times (\frac{x_{ij}-x_{min,j}}{x_{max,j}-x_{min,j}}-0.5)
    $$
+ 稀疏数据spaese data：数据中很多维度值为0
  + 如果所有数据$\geq 0$，在两种归一化中，原来是0的会变成什么？

### 2. $l_2$或$l_1$归一化

+ 若各维度取值范围的不同是有意义的，但是不同数据点之间的大小（如向量长度norm）应该保持一致
  + 对每个数据$\boldsymbol{x}_i=(x_{i1},...,x_{xd})$
    $$
    x_{ij}\leftarrow\frac{x_{ij}}{\lVert\boldsymbol{x}_i\rVert_{l2}}\quad\quad \lVert\boldsymbol{x}_i\rVert_{l2}=\sqrt{\boldsymbol{x}_i^\top\boldsymbol{x}_i}
    $$
+ $l_1$归一化
  + 适用于非负的特征，即$x_{ij}\geq0$总成立
  + 若数据 $\boldsymbol{x}_i$ 是直方图时，经常是最佳的  
    $$
    x_{ij}\leftarrow\frac{x_{ij}}{\lVert\boldsymbol{x}_i\rVert_{l1}}\quad\quad \lVert\boldsymbol{x}_i\rVert_{l1}=\sum_{j=1}^d|x_{ij}|
    $$

### 3. zeor-norm, unit variance

+ 有时候有理由相信每一个维度是服从高斯分布的
  + 希望每一个维度归一化到$N(0,1)$
+ 对每一维$j$，其数据为$x_{1j},x_{2j},...,x_{nj}$
  + 计算其均值$\hat{\mu}_j$和方差$\hat{\sigma}_j^2$
  + 对每一个特征值
    $$
    x_{ij}\leftarrow\frac{x_{ij}-\hat{\mu}_j}{\hat{\sigma}_j}
    $$

### 归一化测试数据

+ 怎样归一化测试数据？`错误！！！`
  + 从测试集寻找最大值、最小值、均值？
+ 除了在测试的时候，永远不要使用测试数据！
  + 测试集和训练集应该使用相同的归一化方法
  + 这个原则同样适用于交叉验证
+ 那么，怎么做？
  + 保存从训练集上取得的归一化参数
  + 使用同样的公式和保存的参数来归一化测试集

### 归一化小结

+ 归一化的方法应该是根据数据的特点来选择的
  + 在做任何机器学习之前，先搞清你的数据的特点
    + 稀疏？
    + 每一维有没有含义？
    + 每一维里面值得分布情况？Gauss？
    + 看你的数据！Do visualization
+ 归一化可能对准确度有极大的影响
  + 在有些例子里，正确的归一化能大幅度提高accuracy
+ 不同的归一化方法可以混合使用

## Fisher线性判别分析（LDA）

### 为什么需要FLD？

+ 理论上可以证明，PCA在数据是单个高斯分布是最佳
  + PCA有利于表示数据，但和分类无关
+ 分类问题中，不同类别的分布$p(\boldsymbol{x}\mid y=i)$不能相同
+ 如何提取特征，最有利于分类？
  + FLD是某些限制条件下最佳的线性特征提取方法
+ PCA是无监督的
{{< img src="5-1.png" alt="Rectangle" class="border-0" >}}

### 用数学形式表示formalize

+ 两个类别$y_i\in\\{1,2\\}$，数据$\boldsymbol{x}_i$，两类各有$N_1,N_2$各点
+ 希望寻找一个投影方向projection direction，$u = \boldsymbol{w}^\top\boldsymbol{x}$，使得两个类别的数据在投影以后容易被分开separate
+ 两个类各自的均值为
  $$
  \boldsymbol{\mu}_1=\frac{1}{N_1}\sum_{y_i=1}\boldsymbol{x}_i\quad\quad \boldsymbol{\mu}_2=\frac{1}{N_1}\sum_{y_i=2}\boldsymbol{x}_i
  $$
+ 投影以后的均值为$m_1=\boldsymbol{w}^\top\boldsymbol{\mu}_1,m_2=\boldsymbol{w}^\top\boldsymbol{\mu}_2$

### Objective: Fisher's Criterion

+ 怎样描述“分开”的程度？
+ Maximize$(m_2-m_1)^2$?问题
  + 这个值可以无限大，怎么解决？
  + 加上限制条件$\boldsymbol{w}^\top\boldsymbol{w}=1$
  + 看前面的图，这个值不是越大越好。怎么解决？
+ Fisher准则
  + 在要求$|m_2-m_1|$尽可能大的同时，要求两类在投影以后尽量集中，或者不分散。怎么度量分散程度？
    $$
    J(\boldsymbol{w})=\frac{(m_2-m_1)^2}{s_1^2+s_2^2}
    $$

### 分散程度的度量

+ 对一维数据，自然的度量是方差或散度(k=1,2)
  $$
  s_k^2=\sum_{y_i=k}(\mu_i-m_k)^2
  $$
  称为类内散度within class scatter
+ $s_1^2+s_2^2$：总的类内散度
+ $s_k^2=\sum_{y_i=k}(\mu_i-m_k)^2=\sum_{y_i=k}(\boldsymbol{w}^\top(\boldsymbol{x}_i-\boldsymbol{\mu}_k))^2=\boldsymbol{w}^\top\sum_{y_i=k}(\boldsymbol{x}_i-\boldsymbol{\mu}_k)(\boldsymbol{x}_i-\boldsymbol{\mu}_k)^\top\boldsymbol{w}$
+ $(m_2-m_1)^2=\boldsymbol{w}^\top(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)^\top\boldsymbol{w}$

### 散布矩阵

+ $S_k=\sum_{y_i=k}(\boldsymbol{x}_i-\boldsymbol{\mu}_k)(\boldsymbol{x}_i-\boldsymbol{\mu}_k)^\top$是什么？
+ 类内散布矩阵
  $$
  S_W=S_1+S_2
  $$
+ 类间散布矩阵
  $$
  S_B=(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)^\top
  $$
+ Fisher准则的矩阵形式
  $$
  \max J(\boldsymbol{w})=\frac{\boldsymbol{w}^\top S_B\boldsymbol{w}}{\boldsymbol{w}^\top S_w\boldsymbol{w}},\ s.t.\ \boldsymbol{w}^\top\boldsymbol{w}=1
  $$
  这种形式称为广义瑞利商

### Optimization：如何求解？

+ 练习：用拉格朗日乘子法，证明（记得查表）最优化时必须满足
  $$
  S_B\boldsymbol{w}=\lambda S_W\boldsymbol{w}
  $$
+ 该问题称为广义特征值问题
  + 得到$S_B$和$S_W$的广义特征值和广义特征向量
+ 但是我们不用去解这个问题
  + $S_B\boldsymbol{w}=(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)^\top\boldsymbol{w}$与$\boldsymbol{\mu}_2-\boldsymbol{\mu_1}$成比例，将这个比例放入$\lambda$中，转化问题为
  + $(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)=\lambda S_W\boldsymbol{w}$

### FLD的步骤

1. 计算$\boldsymbol{\mu}_2,\boldsymbol{\mu}_1$
2. 计算$S_W$
3. 计算$\boldsymbol{w}=S_W^{-1}(\boldsymbol{\mu}_2-\boldsymbol{\mu}_1)$
4. 归一化
   $$
   \boldsymbol{w}\leftarrow\frac{\boldsymbol{w}}{\lVert\boldsymbol{w}\rVert}
   $$

### 如果不可逆怎么办？

+ 如果数据很少或者维度很高，$S_W$很可能不可逆
  + 广义逆矩阵
+ $S_W$是实对称的，而且至少是半正定的
  + $S_W=E\Lambda E^\top,\quad \lambda_{ii}\geq 0$
+ Moore-Penorse伪逆pseudoinverse
  + 若$\lambda_{ii}\geq 0$，定义$\lambda_{ii}^+=1/\lambda_{ii}$，否则定义$\lambda_{ii}^+=0$
  + $\Lambda$的$M-P$伪逆为：$\Lambda^+=\text{diag}(\lambda_{11}^+,...,\lambda_{dd}^+)$
  + $S_W$的伪逆为
    $$
    S_W^+=E\Lambda^+E^\top
    $$

### 如果大于2类怎么办？

+ C类问题
  + $\boldsymbol{\mu}_i,N_i,m_i,S_i$和2类问题中一定定义
  + $S_W=\sum_{i=i}^C S_i$，很容易从2类问题推广
  + 定义$N=\sum_{i=1}^C N_i$
  + 定义总均值$\boldsymbol{\mu}=\frac{1}{N}N_i\boldsymbol{\mu}_i=\frac{1}{N}\sum_x\boldsymbol{x}$
+ $S_B$没有定义，无法直接从2类问题推广
  + 总散布矩阵，$S_T=\sum_x(\boldsymbol{x}-\boldsymbol{\mu})(\boldsymbol{x}-\boldsymbol{\mu})^\top$
  + $S_T=S_W+\sum_{i=1}^C N_i(\boldsymbol{\mu}_i-\boldsymbol{\mu})(\boldsymbol{\mu}_i-\boldsymbol{\mu})^\top=S_W+S_B$
  + 定义多类的$S_B=\sum_{i=1}^C N_i(\boldsymbol{\mu}_i-\boldsymbol{\mu})(\boldsymbol{\mu}_i-\boldsymbol{\mu})^\top$
  + 练习：证明，当$C=2$时，有$S_T=S_W+S_B$

### 更多的投影方向

$$
\max J(w)=\frac{\boldsymbol{w}^\top S_B\boldsymbol{w}}{\boldsymbol{w}^\top S_W\boldsymbol{w}}
$$

+ 求解广义特征值问题
  $$
  S_B\boldsymbol{w}_i=\lambda_i S_W\boldsymbol{w}_i
  $$
+ 最多能得到$C-1$个有效的投影方向
+ 利用Matlab求解

## 应用：人脸识别

### 人脸

+ 为什么人脸数据特别适合PCA和FLD？
+ 用什么分类器？（最近邻）

### 张量Tensor：深度学习的基石

+ 标量：$x\in\mathbb{R}$
+ 向量：$\boldsymbol{x}\in\mathbb{R}^d$
+ 矩阵：$X\in \mathbb{R}^d\times \mathbb{R}^d$
+ 进一步推广？
  + 如果$\mathbb{R}^d\times \mathbb{R}^d\times \mathbb{R}^d$
  + 称为张量tensor，上例是3阶张量
  + 标量、向量、矩阵分别是0、1、2阶张量
+ 张量的操作，最基本的是向量化vectorize
  + 将张量的各行堆积stack起来
