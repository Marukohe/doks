---
title: "10 - 稀疏数据、未对齐数据"
description: ""
lead: ""
date: 2021-04-10T09:54:36+08:00
lastmod: 2021-04-10T09:54:36+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pattern-recognition"
weight: 30
toc: true
---

## 稀疏

### 什么是稀疏

+ 向量$\boldsymbol{x}$
  + 计算其非0元素的个数
+ 那么，图像是稀疏的吗？在什么意义上？
  + 不是在原来的空间（每一维代表一个像素）
  + 而是在某种更有效的（通常是高维但稀疏的）表达方式上
  + 总之，需要正确理解稀疏sparse的含义

## 动态时间弯曲

### Alignment对齐

+ 一个项目是一个时间序列，包含若干个顺序的数据

### 要求

+ 假设两组（顺序放的）数据$\boldsymbol{x}=(x_1,...,x_n),\boldsymbol{y}=(y_1,...,y_m)$
  + 很可能$m\neq n$
  + 对任意的$x_i,y_i$，存在函数$d(x_i,y_i)$描述其距离
  + 如果能找到好的匹配，那么就可以计算$\boldsymbol{x}$和$\boldsymbol{y}$的距离
+ 那么，好的匹配满足什么条件
  + 若$x_i,y_i$匹配，那么$d(x_i,y_i)$越小越好
  + 匹配可以有跳过的数据，如$x_1<->y_2,x_3<->y_3$
    + 可以跳过$\boldsymbol{x}$的，也可以跳过$\boldsymbol{y}$中的数据
  + 匹配时顺序的，如果$x_i<->y_i,x_{i+1}<->y_k$，那么$j\leq k$
  + 选择总距离最小的匹配

### 有问题吗

+ 假设距离$d(x_i,y_i)\geq 0$
  + 如果任何一个数据都不选择进入匹配，那么总距离是？
+ 解决的办法
  + 要求$\forall i,x_i$必须和一个$y_i$匹配，反之亦然
  + 但是，一个$x_i$可以和多个$y_j$匹配，一个$y_i$也可以和多个$x_j$匹配
  + 如$x_1<->y_2,x_1<->y_3,x_2<->y_3$

### 可视化

{{< img src="10-1.png" alt="Rectangle" class="border-0" >}}

### 形式化

+ 匹配变成了发现最佳路径
