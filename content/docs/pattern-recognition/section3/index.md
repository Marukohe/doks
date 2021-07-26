---
title: "3 - 模式识别系统"
description: ""
lead: ""
date: 2021-04-10T09:54:14+08:00
lastmod: 2021-04-10T09:54:14+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 24
toc: true
---

## 最近邻规则

### 问题设置

+ 分类问题
  + 训练集$D=\\{x_1,y_1),(x_2,y_2),\cdots,(x_n,y_n)\\}$
  + 训练样本：$x_i\in X\in\mathbb{R}^d$
  + 样本的标记：$y_i\in Y=\\{1,2,\cdots, C\\}$
+ 存在一个距离函数：$d(x,y)\in\mathbb{R}$
  + 能够度量$x$和$y$之间的距离，或者不相似程度

### 最近邻规则和Voronoi图

+ 给定一个测试样例$x$
  1. 发现其最近邻$i^\*=\argmin\limits_id(x,x_i)$
  2. 输出对$x$的分类预测：$y_{i^\*}$
+ Voronoi图
  {{< img src="3-1.png" alt="Rectangle" class="border-0" >}}

### 最近邻可能出现的问题

+ 如果出现平局(tie)?
+ 如果出现离群点(outlier)?
  + 使用kNN
+ 能做的多好？
  + 当训练样本趋于无穷时，最近邻的错误率做多是最佳错误率的两倍

### 计算、存储代价

+ 假设$d(x,y)$是欧氏距离
  + 其复杂度是$O(d)$
  + $NN$的复杂度$O(nd)$
  + $k-NN$的复杂度同样是$O(nd)$
    + 或者是$O(nd)+O(n)+O(k)$
+ 对存储要求非常高，需要存大量的距离

### 降低$NN$的计算、存储代价

+ 近似最近邻$ANN$
  + 不要求一定是距离最短的$k$个
  + 如第$k$个$NN$，其距离是$d_k$，则$ANN$要求其选取的所有$k$个样例距离$d$满足$d\leq(1+\epsilon)d_k$即可
  + 可以将$kNN$搜索速度提高几个数量级

## 系统各模块（混合）简介

### 细化的框架

+ 获取数据$\rightarrow$提取特征$\rightarrow$进行学习$\rightarrow$评价评估$\rightarrow$实际应用
+ 机器学习$f:\mathcal{X}\rightarrow \mathcal{Y}$
  1. 与领域无关的特征变化和特征抽取
  2. 针对不同数据特点的不同学习算法
  3. 机器学习方法的常见分类、策略
+ 针对不同问题的评价准则

### 评价准则——泛化和测试误差

+ 暂时只考虑分类问题和评价
+ 假设$(x,y)\sim p(x,y)$
  + 泛化误差generalization error：$E_{(x,y)\sim p(x,y)}[f(x)\neq y]$
  + 根本假设：训练集$D_{train}$和测试集$D_{test}$都是服从真实数据分布$p(x)$的，或者，他们的样例都是从$p(x)$中取样的
  + 测试误差
    $$
    err = \frac{1}{n}\sum_{i=1}^n\mathbb{I}(f(x_i)\neq y_i),\ x_i\in D_{test}
    $$
  + 精确度(accuracy)：$acc = 1- err$

### 一种常见的学习框架

+ 代价最小化cost minimization
  + 错误是最常考虑的代价，所以现在我们可以说学习的目标是在训练集上获得最小的代价
+ $\min\limits_f\frac{1}{n}\sum_{i=1}^n\mathbb{I}(f(x_i)\neq y_i),\ x_i\in D_{train}$
  + 难以优化——怎样求解？
  + 一种方法是：把不连续的指示函数换成性质相似的，但好优化的函数
  + 如，$(f(x_i)-y_i)^2$
+ 学习这种思路：形式化、简化、优化

### 过拟合和欠拟合

+ Overfitting & Underfitting
+ 欠拟合：学习模型的复杂性小于数据的复杂性
+ 过拟合：学习模型的复杂性大于数据的复杂性

### 正则化

+ 通常难以精确估计学习模型、数据的复杂性
  + 往往选用较复杂的学习模型
  + 训练集误差通常小于测试集误差
+ 那么如何降低Overfitting的可能性呢？
  + 正则化，会在SVM部分看到例子

### 如果没有测试集

+ 例如，总的数据量比较小
+ 交叉验证
  1. 将训练集分为大小大致相等的N部分
  2. for i = 1:N
     1. 取第$i$部分的数据为测试集
     2. 取所有其余的数据为训练集
     3. 学习模型并评估/测试得到的错误率$err_i$
  3. 交叉验证得到的错误率为$\frac{1}{N}\sum err_i$
  4. 称为$N$倍交叉验证$N-fold\ CV$（常用N=5或10）
+ 可能需要多次试验

### 数据、代价的不平衡性imbalance

+ 例如，两类问题中，一类数据远比另一类数据多
  + 体检中的阴性和阳性：数据不均衡
  + 在一类犯错的代价远高于另一类：代价不均衡
+ 不平衡学习
+ 代价敏感学习

### 评价不平衡时的准则

|      |预测为positive |预测为negative|
|------|-------|--------|  
|真实值为positve | True positive(真阳性)|False negative(伪阴性)|
|真实值为negative| False positive(伪阳性)| True negative(真阴性)|
{.table-striped}

+ TP、TN、FP、FN：标记四种情况的样例数目
+ TOTAL：总数TP+TN+FP+FN
  + 正样本数目：P=TP+FN，负样本数目：N=FP+TN
+ False positive rate: FPR = FP / N
+ False negative rate: FNR = FN / P
+ True positive rate: TPR = TP / P
+ Accuracy: ACC = (TP + TN) / TOTAL
+ AUC-ROC(Area Under the ROC Curve)
  + ROC - Receiver operating characteristic
+ Precision(查准率)：PRE = TP / (TP + FP)
+ Recall(查全率)：REC = TP / P（和TPR一样）
+ F1 score：Precision和Recall的调和平均
  + 调和平均：$(\frac{x^{-1}+y^{-1}}{2})^{-1}=\frac{2xy}{x+y}$
  + F1 = 2TP / (2TP+FP+FN)
+ AUC-PR(Area Under the Precision-Recall Curve)
  + 比AUC-ROC更有区分力
  + 非单调

### 代价矩阵

+ 目前常见的为
  $$
  \left(\begin{matrix}
  \lambda_{11}&\lambda_{12}\cr
  \lambda_{21}&\lambda_{22}
  \end{matrix}\right)=
  \left(\begin{matrix}
    0&1\cr
    1&1
  \end{matrix}\right)
  $$
  + $\lambda_{ij}$：当前真实值为$i$、模型预测为$j$时付的代价
  + $0-1$代价：即分类正确代价为0，分类错误代价为1
  + 但是，根据实际情况，可以给$\lambda_{ij}$设置任何值
    + 代价不平衡学习
+ 对代价的计算：$E_{(x,y)}[\lambda_{y,f(x)}]$
  + 当使用0-1代价时，和错误率一致

### 真实值Groundtruth

+ 大多数时候，是手工标注的（manual annotation）
  + 或者人也不知道确切的答案
  + 有时候疲劳或其他因素会导致标注的错误
  + 很耗时、昂贵
+ 真实值的形式
  + 分类：一个离散的类别
  + 回归regression：一个连续的值
  + 结构structured output：例如，输出一个句子的分词结果“一个/句子/的/分词/结果”
  
### 能100%准确吗：Bayes框架的回答(1)

+ $f:\mathcal{X}\mapsto\mathcal{Y}$，一个数据对(data pari)：$(\mathcal{x}, \mathcal{y})$
  + 假设注重于分类：$\mathcal{Y}=\\{1,2,...,m\\}$
  + 先验概率prior probability：$p(y=i)$
  + 后验概率posterior probability：$p(y=i\mid x)$
  + 类条件概率class conditional probability：$p(x\mid y = i)$
+ 贝叶斯定理 Bayes's theorem
  $$
  p(y=i\mid x) = \frac{p(x\mid y= i)p(y=i)}{p(x)}=\frac{条件\times 先验}{数据}
  $$

### 能100%准确吗：Bayes框架的回答(2)

+ 贝叶斯决策规则
  + 选择代价最小的类别输出
    $$
    \argmin_iE_{(x,y)}[\lambda_{y,f(x)}]
    $$
  + 贝叶斯风险：使用贝叶斯决策规则的风险
  + 其是理论上我们能得到的最好的结果，记为$R^*$
+ 在使用0-1风险时，风险和错误率等价
  + 所以，$R^*$时我们理论上能得到的最小误差
  + 1-$R^*$是理论上最高的准确率

### 贝叶斯决策规则

+ 在0-1风险时，选择后验概率最大的那个类别$\argmax_ip(y=i\mid x)$

### 错误从哪里来——以回归为例？

+ 真实（但未知）的函数$F(x)$
  + 用尤其产生的数据集$D$来学习，即$y=F(x)$没有误差
  + 回归的代价函数是欧几里得距离
+ 偏置方差分解：误差平方 = bias平方+预测方差
  $$
  E_D[(f(x;D)-F(x))^2]=(E_D[f(x;D)]-F(x))^2+E_D[(f(x;D)-E_D[f(x;D)])^2]
  $$
  + $x$和$F(x)$是定值，只有$D$出现时才取期望
  + 简写为$E[(f-F)^2]=(F-Ef)^2+E[(f-Ef)^2]$

### 偏置-方差分解

+ Bias-varience decomposition
  + $E[(f-F)^2]=(F-Ef)^2+E[(f-Ef)^2]$
  + $E[F-Ef]$--偏置
    + 当训练集取值有差异时，其值不变
  + $E[(f-Ef)^2]=Var_D(f(x;D))$方差
    + 当训练集取样有差异时，会带来预测的差异
+ 误差=$偏置^2$+方差
+ 当考虑到$y=F(x)$有误差时（白噪声）
  + 误差=$偏置^2$+方差+噪声
  + 估计误差时，如果没有测试集，需多次平均

### 对分解的解读

+ 偏置于数据无关，是由模型（的复杂度）决定的
  + 例如，线性分类器（1阶多项式）的偏置大
  + 但是，7阶多项式的复杂度高，偏置小
+ 但是，方差$Var_D(f(x;D))$和抽样得到的训练集以及模型两者都有关系
  + 例如，高阶多项式的方差大
+ 怎么减少误差？
  + 对于噪音，机器学习没有办法——高质量的数据获取
  + 减少偏置和方差
    + 如集成方法
