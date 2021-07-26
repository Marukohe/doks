---
title: "8 - 度量，信息论，决策树"
description: ""
lead: ""
date: 2021-04-10T09:54:28+08:00
lastmod: 2021-04-10T09:54:28+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 29
toc: true
---

## 度量

### 特征的表示和比较

+ 两个重要的任务：
  + 特征的表示：特征抽取后，如何表示为数学化或者计算机可以理解的数据形式？
    + 到目前为止：所有数据均表示为一个连续的实数值的向量$x\in\mathbb{R}^d$
  + 特征的比较：比较两个点的相似性
    + 在NN、线性分类器、SVM中到目前为止是用欧氏距离
    + 在概率方法中，如高斯分布和KDE，也是欧氏距离
+ 对这些数据（实数向量、可以计算距离或相似程度），成为matric data
+ 但是：还有很多其它类型的数据

### 更多的数据类型

+ 标记数据Normal data
  + 如数据1,2,3分别表示苹果、梨和香蕉
  + 不是连续的实数值，也不可以比较大小（1<2代表苹果不如梨吗？）、不可以比较相似性
+ 时间序列数据time series data
  + 如一个序列(63,64,62)是单个样例，表示某人今天早中晚测量的体重；(61,65)是第二天早晚的题重
  + 不是向量，测量次数不等，如何比较？

### 更多的度量

+ 目前已用
  + 不相似程度或距离：欧氏距离
  + 相似程度：内积或者RBF核
  + 两种紧密联系
+ 但是，数据的不同特点要求使用不同的度量
+ 度量metric

### Metric

+ 一个度量$d$必须满足：对任意向量$\boldsymbol{x},\boldsymbol{y},\boldsymbol{z}$
  + 非负：$d(\boldsymbol{x},\boldsymbol{y})\geq 0$
  + 自反：$d(\boldsymbol{x},\boldsymbol{y})=0$当且仅当$\boldsymbol{x}=\boldsymbol{y}$
  + 对称：$d(\boldsymbol{x},\boldsymbol{y})=d(\boldsymbol{y},\boldsymbol{x})$
  + 三角不等式：$d(\boldsymbol{x},\boldsymbol{y})+b(\boldsymbol{y},\boldsymbol{z})\geq b(\boldsymbol{x},\boldsymbol{z})$

### 从欧氏距离到度量学习

+ Euclidean distance：$d^2(\boldsymbol{x},\boldsymbol{y})=(\boldsymbol{x}-\boldsymbol{y})^\top(\boldsymbol{x}-\boldsymbol{y})$
+ Mahalanobis distance：$d^2(\boldsymbol{x},\boldsymbol{y})=(\boldsymbol{x}-\boldsymbol{y})^\top\Sigma^{-1}(\boldsymbol{x}-\boldsymbol{y})$
  + $\Sigma$是数据的协方差矩阵
+ 进一步推广：可以用一个半正定的矩阵$A$代替$\Sigma^{-1}$
  + $d_A^2(\boldsymbol{x},\boldsymbol{y})=(\boldsymbol{x}-\boldsymbol{y})^\top A(\boldsymbol{x}-\boldsymbol{y})$
  + $A$半正定，存在$G$，使得$A=G^\top G$
  + 因此，$d_A^2(\boldsymbol{x},\boldsymbol{y})=\lVert G\boldsymbol{x}-G\boldsymbol{y}\rVert^2_2$

### 固定形式的distance

+ Minkowski distance：$d_p(\boldsymbol{x}-\boldsymbol{y})=(\sum_{i=1}^d|x_i-y_i|^p)^{\frac{1}{p}}$
  + $p\geq 1$时是metric
  + $p=2$是欧氏距离，成为曼哈顿距离
  + 若$p<1$，不是metric

### Norm、distance、similarity

+ 一个向量$\boldsymbol{x}$的$p$ norm（或者$L_p$ norm）
  $$
  \lVert\boldsymbol{x}\rVert_p=\left(\sum_{i=1}^d|x_i|^p\right)^{\frac{1}{p}}
  $$
  + 限制条件$p\geq 1$
    + $\lVert \boldsymbol{x}\rVert_\infty=\max(|x_1|,...,|x_d|)$
+ 距离和长度的关系：$d_p(\boldsymbol{x},\boldsymbol{y})=\lVert\boldsymbol{x}-\boldsymbol{y}\rVert_p$
+ 从距离到相似度，例如
  $$
  \exp(-\gamma\lVert\boldsymbol{x}-\boldsymbol{y}\rVert_p)
  $$

### 幂平均函数

+ 幂平均
  + $M_p(x_1,...,x_n)=\left(\frac{1}{n}\sum_{i=1}^n x_i^p\right)^{\frac{1}{p}}$
  + 对$p$在整个实数轴上都有定义
    + $M_{-\infty}=min(x_1,...,x_n)$
    + $M_{-1}$调和平均
    + $M_0=\sqrt[n]{x_1,...,x_n}$几何平均
    + $M_1$算术平均
    + $M_2$ root mean square
    + $M_{\infty}=max(x_1,...,x_n)$
  + 若$p<q$，则$M_p(x_1,...,x_n)\leq M_q(x_1,...,x_n)$

### 幂平均核

$$
M_p(\boldsymbol{x},\boldsymbol{y})=\sum_{i=1}^d M_p(x_i,y_i)
$$

+ 当$p\leq 0$时，以上函数为Mercer核
+ 属于加性核
  + $p=0,\sqrt{x_iy_i}$ —— Hellinger's Kernel
  + $p=-1,\frac{2x_iy_i}{x_i+y_i}$ ——$\chi^2$核
  + $p=-\infty,min(x_i,y_i)$ —— histogram intersection核
+ 当特征是直方图时，加性核效果极佳

## Nominal data(标记数据)

### 标记数据的比较

+ 标记数据Nominal data
  + 如果数据1,2,3分别表示苹果、梨和香蕉，怎么比较？
+ 基本思想：相同则为1，否则为0，即两个标记数据$x$和$y$的相似程度为$\text{II}(x=y)$
+ 度量化
  + 设标记数据可以取$m$个不同的值，标记为$\\{1,2,...,m\\}$
  + 将标记数据$x=i$转换成一个向量$(\boldsymbol{0},...,1,...,\boldsymbol{0})$
  + 假设$x,y$转换为$\boldsymbol{x},\boldsymbol{y}$，那么$\text{II}(x=y)=\boldsymbol{x}^\top\boldsymbol{y}$
  + SVM即可用该方法处理标记数据

### 从度量化到直方图

+ 可以看成，度量化的过程是将一个标记数据转化成为一个所有可能取值的直方图
  + 一个直方图histogram是对一个集合中元素的计数
  + 若$x=i$，其度量化的结果$\boldsymbol{x}$为$m$个bin的直方图
  + 第$i$个bin值为1，表示有一个样例取值为$i$
  + 其余所有bin为0，表示没有任何样例取值这些值
+ 那么， 假设有两组数据，直方图分别为$\boldsymbol{x}$和$\boldsymbol{y}$
  + 应该怎么计算其相似性
  + $\min(x_i,y_i)$

## 信息论简介

### 从直方图到概率分布

+ 在非参数估计中，我们怎么估计一个分布
  + 最早从直方图开始
+ 那么我们怎么比较两个分布呢？
  + 假设$p$和$q$是两个离散分布，那么HIK可以用吗？怎么用？
  + 如果是连续分布呢？有没有理论上完备的方法？
  + 信息论！Information theory

### 信息information

+ 描述一个随机变量需要多少信息？
  + 假设用bit来作为信息的单位
  + 若离散变量满足$P(x=2)=1,P(x\neq 2)=0$
  + 若离散变量是$\\{1,2,3,4\\}$上的均匀分布
+ 熵entropy
  + $H=-\sum_{i=1}^m P_i\log_2 P_i$(m个离散可能取值，各位$P_i$)
  + 如果$P_i=0$
    + 定义$0\log_20=0$
  + 什么时候最大，什么时候最小？
    + 均匀分布的时候最大，$\log_2 m$
    + 单点分布最小，0

### Differential entropy(非负性不存在了)

+ 如果分布是连续的？
  $$
  h(x)=-\int p(x)\ln(p(x))dx
  $$
  + 自然对数，单位是nat（奈特）
  + 若$X\sim N(\mu,\sigma^2)$，则$h(x)=\frac{1}{2}\ln (2\pi e\sigma^2)$ nats
+ 在所有均值和方差固定的连续分布中，高斯分布具有最大的熵
  + 或者说，不确定性最大

### Joint,conditional entropy

+ $H(X,Y)=-\sum_x\sum_yP(x,y)\log_2 P(x,y)$
+ $h(X,Y)=-\int p(x,y)\ln p(x,y) dxdy$
+ $H(X\mid Y)=\sum_yp(y)H(X\mid Y=y)=\sum_{x,y}P(x,y)\log_2\frac{P(y)}{P(x,y)}=-\sum_{x,y}P(x,y)\log_2\frac{P(x,y)}{P(y)}$
+ $h(X\mid Y)=-\int p(x,y)\ln p(x\mid y)dxdy=-\int p(x,y)\ln\frac{p(x,y)}{p(y)}dxdy$

### 各种熵之间的关系

{{< img src="8-1.png" alt="Rectangle" class="border-0" >}}

### 互信息

+ 如果$X$和$Y$互相独立，即$p(x,y)=p(x)p(y)$，或者$P(x,y)=P(x)P(y)$
  + 上面的图怎么画？
  + $I(X;Y)$表示$X$和$Y$共同的那部分信息
    $$
    I(X;Y)=H(X)-H(X\mid Y)=\sum_{x,y}p(x,y)\log_2 \frac{p(x,y)}{p(x)p(y)}=H(Y)-H(Y\mid X)
    $$
+ $I(X;Y)=I(Y;X)$
+ 可以粗略的看成相似程度或者相关程度

### KL散度

+ Kullback-Leibler divergence：两个离散分布$P$和$Q$
  $$
  D_{KL}(P\lVert Q)=\sum_{i} P_i\log_2\frac{P_i}{Q_i}
  $$
  + $D_{KL}(P\lVert Q)\geq 0$，等号当且仅当$\forall i, P_i=Q_i$时成立
  + $I(X;Y)=D_{KL}(p(x,y)\lVert p(x)p(y))$
  + 可以粗略看成“距离”
  + 但是，KL散度对称吗？（不对称）

## 决策树

### Titanic survivors

+ 该判断模型是树tree
+ 每次根据一个数据（成为属性）分成若干部分
+ 当不可再分时（叶节点），给出一个决策decision
  + 通常输出的决策是标记数据
  + 可以输出一个概率分布

### 那么，选哪个属性来分？

+ 问题的输出是标记数据，有$m$个可能的值
+ 如果当前节点一共包含$n$个样例，记为集合$T$
+ 其对应样例的groundtruth输出是集合$y_T$
+ 计算$H(y_T)$——当前节点的不纯度$impurity$
+ 对每一个属性$j$
  + 其不同值将当前节点数据分为若干子集$T_1,T_2...$
  + 计算每个子集的entropy：$H(y_{T_k})$和比例$w_k$
  + 计算按此属性分开后的平均不纯度 $\sum_{k}w_kH(y_{T_k})$
  + Information gain信息增益：$H(Y_T)-\sum_kw_k H(Y_{T_k})$
+ 选择信息增益最大的那个属性
