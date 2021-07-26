---
title: "6 - 支持向量机SVM"
description: ""
lead: ""
date: 2021-04-10T09:54:22+08:00
lastmod: 2021-04-10T09:54:22+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 27
toc: true
---

## 统计学习方法的粗略分类

+ Statistial learning methods
  + $p(y=i),\ p(y = i\mid \boldsymbol{x}),\ p(\boldsymbol{x}\mid y = i),\ p(\boldsymbol{x})$
  + Generative (probabilistic) medels：估计$p(\boldsymbol{x}\mid y = i)$和$p(y)$
    + 然后用贝叶斯定理求$p(y=i\mid\boldsymbol{x})$
    + 生成模型
  + Discriminative (probabilistic) methods：直接估计$p(y=i\mid\boldsymbol{x})$
    + 判别模型
  + Discriminant function：直接求一个把各类分开的边界
    + 不假设概率模型：如FLD，SVM

## large margin（最大边际？）

+ 用线性边界分开2类
  + 正类，$y_i=1$
  + 负类，$y_i=-1$
  + 可以有很多边界$L1,L2,L3,...$，在训练集上都100%正确，哪个更好？
  {{< img src="6-1.png" alt="Rectangle" class="border-0" >}}

## margin

{{< img src="6-2.png" alt="Rectangle" class="border-0" >}}

+ 一个点（样例）的边际margin是其到分界超平面separating hyperplane的垂直距离
+ SVM最大化（所有训练样本的）最小边际
+ 有最小边际的点称为支持向量(support vectors)
  + 所以叫支持向量机(support vector machine)

## 几何geometry示意图

+ 分类超平面
  $$
  f(\boldsymbol{x}) = \boldsymbol{w}^\top \boldsymbol{x}+b = 0
  $$
  + 红色
  + 绿色为其法向量
+ x为任一点/样例，其到超平面的距离为？
{{< img src="6-3.png" alt="Rectangle" class="border-0" >}}

## 计算margin

+ 投影点为$\boldsymbol{x}_\perp$，$\boldsymbol{x} - \boldsymbol{x}_\perp$为距离向量
  + 其方向与$\boldsymbol{w}$相同，为$\frac{\boldsymbol{w}}{\lVert\boldsymbol{w}\rVert}$
  + 其大小$r$可为0，或正，或负；margin为其大小的绝对值
+ $\boldsymbol{x}=\boldsymbol{x}_\perp+r\frac{\boldsymbol{w}}{\lVert\boldsymbol{w}\rVert}$，两边同时乘以$\boldsymbol{w}^\top$，然后加上$b$
  + $\boldsymbol{w}^\top\boldsymbol{x}+b=\boldsymbol{w}^\top \boldsymbol{x}_\perp+b+r\frac{\boldsymbol{w}^\top\boldsymbol{w}}{\lVert\boldsymbol{w}\rVert}$
  + $f(\boldsymbol{x}) = f(\boldsymbol{x}_\perp)+r\lVert\boldsymbol{w}\rVert$
  + $r=\frac{f(\boldsymbol{x})}{\lVert\bold{w}\rVert}$ (因为$\boldsymbol{x}_\perp$在超平面上，结果为0)
  + $\boldsymbol{x}$的margin是$\frac{|f(\boldsymbol{x}|}{\lVert\boldsymbol{w}\rVert}=\frac{|\boldsymbol{w}^\top\boldsymbol{x}+b|}{\lVert\boldsymbol{w}\rVert}$

## 分类、评价

+ 怎么样分类？
  + $f(\boldsymbol{x})>0$ 分为正类，$f(\boldsymbol{x})<0$分为负类
  + 那么$f(\boldsymbol{x})=0$怎么办？
+ 对于任何一个样例，怎么直到预测的对错
  + $y_if(\boldsymbol{x_i})>0$正确，$y_if(\boldsymbol{x_i})<0$错误
  + 即，因为我们假设能完全分开，所以
    $$
    y_if(\boldsymbol{x}_i)=|f(\boldsymbol{x}_i)|
    $$

## SVM的形式化描述

+ 那么，SVM的问题是什么？
  $$
  \argmax_{\boldsymbol{w},b}(\min_i(\frac{|\boldsymbol{w}^\top\boldsymbol{x}_i+b|}{\lVert\boldsymbol{w}\rVert}))
  $$
  $$
  \argmax_{\boldsymbol{w},b}(\min_i(\frac{y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b)}{\lVert\boldsymbol{w}\rVert}))
  $$
  $$
  \argmax_{\boldsymbol{w},b}(\min_i(\frac{1}{\lVert\boldsymbol{w}\rVert}{y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b)}))
  $$
+ 非常难以优化，怎么办？
  + 继续化简

## 换个角度看问题

+ 到目前为止
  + 对$\boldsymbol{w}$没有限制，要求最大化最小的边际，难优化
+ 判断对错，如果$yf(\boldsymbol{x})>0$即正确
  + 即$y(\boldsymbol{w}^\top\boldsymbol{x}+b)>0$，只需要方向，不需要大小
  + 如果$(\boldsymbol{w},b)$变为$(\lambda\boldsymbol{w},\lambda b)$，预测和边际会变吗？
+ 那么我们可以限定$\min_i(y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b))$为1
  + 问题变为：在限制 $\min_i(y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b))$ 为1时，最大化 $\frac{1}{\lVert\boldsymbol{w}\rVert}$
  
    $$
    \argmin_{\boldsymbol{w},b}\frac{1}{2}\boldsymbol{w}^\top\boldsymbol{w}\newline
    s.t.\ \min_i(y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b))\geq 1,\forall i
    $$

## 拉格朗日乘子法

+ $L(\boldsymbol{w},b,\boldsymbol{a})=\frac{1}{2}\boldsymbol{w}^\top\boldsymbol{w}-\sum_{i=1}^na_i(y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b)-1)$
  + subject to $a_i\geq 0$
+ （作业：证明最优化的必要条件）
+ $\frac{\partial L}{\partial\boldsymbol{w}}=0\rightarrow \boldsymbol{w}=\sum_{i=1}^na_iy_i\boldsymbol{x}_i$
+ $\frac{\partial L}{\partial b}=0\rightarrow 0 = \sum_{i=1}^na_iy_i$
+ 在此两条件下，将两个等式带入回$L$
  $$
  \tilde{L}(\boldsymbol{a}) = \sum_{i=1}^n a_i-\frac{1}{2}\sum_{i=1}^n\sum_{j=1}^n a_ia_jy_iy_j\boldsymbol{x}_i^\top \boldsymbol{x}_j
  $$

## SVM的对偶形式

+ 在原来的空间（输入空间）中
  + 变量是$\boldsymbol{x}$，称为SVM的primal form
+ 现在的问题里面
  + 变量是$a_i$，即拉格朗日乘子，称为对偶空间dual space
  + 对偶空间完成优化后，得到最优的$\boldsymbol{a}$，可以得到原始空间中的最优解$\boldsymbol{w}$
+ SVM的对偶形式dual form
  $$
  \argmax_{\boldsymbol{a}}\sum_{i=1}^n a_i-\frac{1}{2}\sum_{i=1}^n\sum_{j=1}^n a_ia_jy_iy_j\boldsymbol{x}_i^\top \boldsymbol{x}_j\newline
  s.t.\  a_i\geq 0\ \sum_{i=1}^n a_iy_i=0
  $$

## 剩下的问题

+ 如何最优化？
  + 对偶空间中
  + 原始空间中
+ 如果能允许少数点$y_if(\boldsymbol{x}_i)<1$
  + 如果允许一个点$y_if(\boldsymbol{x}_i)<1$，但是大幅度增加margin呢？
+ 如果不是线性可分的linearly separable，但是可以用非线性的边界分开non-linearly seperable？
+ 如果不是两个类，而是多个呢？

## Soft margin

+ 可以允许少数点margin比1小
  + 但是犯错误是有惩罚点的，否则？
  + $y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b)\geq 1\rightarrow y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b)\geq 1-\xi_i$
  + $\xi_i$：松弛变量slack variable，即允许犯的错误
  + $\xi_i\geq 0$
  + $=0,(0,1),=1,>1$各自代表什么？
  {{< img src="6-4.png" alt="Rectangle" class="border-0" >}}

## 如何惩罚？

+ Primal space
  $$
  \argmin_{\boldsymbol{w},b}\frac{1}{2}\boldsymbol{w}^\top\boldsymbol{w}+C\sum_{i=1}^n\xi_i\newline
  s.t.\ y_i(\boldsymbol{w}^\top\boldsymbol{x}_i+b)\geq 1-\xi_i,\forall i\newline
  \quad\quad\xi_i\geq 0
  $$
+ $C>0$：正则化参数
  + $\xi_i$——代价，我们要最小化代价函数
  + $\frac{1}{2}\boldsymbol{w}^\top\boldsymbol{w}$——正则项，对分类器进行限制，使复杂度不至于太高（另一个角度，还是最大化边际）
  + 那么，怎么确定C的值？

## Soft margin的对偶形式

+ 推导见PRML
  $$
  \argmax_{\boldsymbol{a}}\sum_{i=1}^n a_i-\frac{1}{2}\sum_{i=1}^n\sum_{j=1}^n a_ia_jy_iy_j\boldsymbol{x}_i^\top \boldsymbol{x}_j\newline
  s.t.\  C\geq a_i\geq 0\ \sum_{i=1}^n a_iy_i=0
  $$
+ 对偶形式仅依赖于内积

## 内积：线性和非线性的联系

+ 线性和非线性有时候紧密联系在一起——通过内积
+ $\boldsymbol{x} = (x_1,x_2),\boldsymbol{z}=(z_1,z_2)$
+ $K(x,z)=(1+\boldsymbol{x}^\top\boldsymbol{z})^2=(1+x_1z_1+x_2z_2)^2$

## Kernel trick

+ 两个向量$\boldsymbol{x},\boldsymbol{y}\in\mathbb{R}^d$，一个非线性函数$K(\boldsymbol{x},\boldsymbol{y})$
+ 对于满足某些条件的函数$K$，一定存在一个映射$\phi:\mathbb{R}\rightarrow\Phi$，使得对任意的$\boldsymbol{x},\boldsymbol{y}$
  $$
  K(\boldsymbol{x},\boldsymbol{y})=\phi(\boldsymbol{x})^\top\phi(\boldsymbol{y})
  $$
  + 非线性函数$K$表示两个向量的相似函数
  + 其等价于$\Phi$里面的内积
+ $\Phi$：特征空间
  + 可以是有限维德空间，但也可以是无穷维的空间

## 什么样的限制条件？

+ 必须存在特征映射，才可以将非线性函数表示为特征空间中的内积
+ Mercer's condition（充分必要）：对任何满足 $\int g^2(\boldsymbol{u})d\boldsymbol{u}<\infty$ 的非零函数，对称函数$K$满足条件：$\int\int g(\boldsymbol{u})K(\boldsymbol{u},\boldsymbol{v})g(\boldsymbol{v})d\boldsymbol{u}d\boldsymbol{v}\geq 0$
+ 另一种等价形式：对任何一个样本集合$\{\boldsymbol{x}_1,...,\boldsymbol{x}_n,\boldsymbol{x}_i\in\mathbb{R}^d\}$，如果矩阵
  $K=[K_{ij}]_{i,j}$（矩阵的第$i$行、第$j$列元素 $K_{ij}=K(\boldsymbol{x}_i,\boldsymbol{x}_j)$ ）总是半正定的，那么函数$K$满足Mercer条件。

## 核支持向量机Kernel SVM

+ 核函数kernel function：K
+ 对偶形式：
  $\argmax_{\boldsymbol{a}}\sum_{i=1}^n a_i-\frac{1}{2}\sum_{i=1}^n\sum_{j=1}^na_ia_jy_iy_jK(\boldsymbol{x}_i,\boldsymbol{x}_j)$
+ 分类边界：$\boldsymbol{w}=\sum_{i=1}^na_iy_i\phi(\boldsymbol{x}_i)$
+ 怎样预测：$\boldsymbol{w}^\top\phi(\boldsymbol{x})=\phi(\boldsymbol{x})^\top(\sum_{i=1}^na_iy_i\phi(\boldsymbol{x}_i))=\sum_{i=1}^na_iy_iK(\boldsymbol{x},\boldsymbol{x}_i)$
  + 线性：$\boldsymbol{w}=\sum_{i=1}^na_iy_i\boldsymbol{x}_i$，$\boldsymbol{w}^\top\boldsymbol{x}$计算量为$O(d)$
  + 非线性（核）方法测试所需时间为？
  + 假设计算$K$的时间为$O(d)$，是$O(nd)$吗？

## Complementary Slackness

+ 对所有$i$，KKT条件包括$(C-a_i)\xi_i=0$
  + 情况1：$C>a_i>0,\xi_i=0$，在特征空间中边际为1的两个超平面上
  + 情况2：$a_i=C$，对$\xi_i$没有限制
    + 可以在超平面上、介于两个超平面之间、或以外（即分类错误）
  + 情况3：$a_i=0$，在预测时不需要计算
+ 这表明
  + 复杂度由$a_i>0$的个数，而非样本的总数目来决定的
  + $\boldsymbol{a}$是稀疏的
  + 在soft margin SVM中，称$a_i>0$对应的$\boldsymbol{x}_i$为支持向量

## 非线性核

+ 线性核：$K(\boldsymbol{x},\boldsymbol{y})=\boldsymbol{x}^\top\boldsymbol{y}$
+ 非线性核：
  + RBF、高斯核：$K(\boldsymbol{x},\boldsymbol{y})=\exp(-\gamma\lVert\boldsymbol{x}-\boldsymbol{y}\rVert^2)$
  + 多项式核：$K(\boldsymbol{x},\boldsymbol{y})=(\gamma\boldsymbol{x}^\top\boldsymbol{y}+c)^d$

## 超参数

+ 如何决定$C,\gamma,...$
  + 必须给定这些参数的值，才能进行SVM学习，SVM本身学习这些参数！
  + 称为超参数
  + 对SVM的结果有极大的影响
+ 用交叉验证在训练集上来学习
  + 在训练集上得到不同参数的交叉验证准确率
  + 选择准确率最高的超参数的值

## 多类(1)

+ 思路：转化为2类问题
+ 1-vs-1：C个类$\\{1,2,...,C\\}$
  + 设计$C\choose 2$个分类器：用$i$和$j(i>j)$两类的训练数据学习
  + 一共$C(C-1)/2$个，其中每个类出现$C$次
  + 对测试样本$\boldsymbol{x}$，一共会得到$C(C-1)/2$个结果，然后投票
  + 每个分类器$f_1$采用其二值输出，即$sign(f_i(\boldsymbol{x}))$
  {{< img src="6-5.png" alt="Rectangle" class="border-0" >}}

## 多类(2)

+ 1-vs.-all（或1-vs.-rest）
  + 设计$C$个分类器，第$i$个分类器用类$i$做正类，把其他所有$C-1$个类别的数据合并在一起做负类
    + 和交叉验证的步骤有些类似
    + 每个新的分类器$f_i$采用其真是数值输出，即$f_i(\boldsymbol{x})$
  + $f_i(\boldsymbol{x})$的实数输出可以看成是其“信心”
  + 最终选择信心最高的那个类输出
