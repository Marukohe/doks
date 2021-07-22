---
title: "随机变量的数字特征"
description: ""
lead: ""
date: 2021-04-10T11:01:53+08:00
lastmod: 2021-04-10T11:01:53+08:00
draft: false
images: []
menu: 
  docs:
    parent: "pms"
weight: 34
toc: true
---

## 1. 数学期望

+ 定义: 设离散型随机变量$X$的分布率为$P(X=x_i),i = 1,2...$，若级数$\sum\limits_{i=1}^{+\infty}|x_i|p_i$收敛，则称$\sum\limits_{i=1}^{+\infty}|x_i|p_i$为$X$的数学期望，记为$EX$，即
  $$
  EX=\sum\limits_{i=1}^{+\infty}x_ip_i
  $$

+ 定义: 设连续型随机变量$X$的密度为$p(x)$，若积分$\int_{-\infty}^{+\infty}|x|p(x)dx<\infty$，则定义$X$的数学期望$EX$为
  $$
  EX=\int_{-\infty}^{+\infty}xp(x)dx
  $$

### 常见分布的数学期望

1. 0-1分布(两点分布)$B(p)$

   设随机变量$X$服从0-1分布:
   $$
   X\sim\left(\begin{matrix}
   0&1\cr
   q&p
   \end{matrix}\right)
   , 0<p<1,\ p+q=1
   $$
   则$EX = 0\cdot q+1\cdot p = p$.

   $D(X) = pq​$

   

2. 二项分布$B(n,p),0<p<1$

   设随机变量$X$服从二项分布:
   $$
   P(X=k) = C_n^kp^k(1-p)^{n-k},\quad k = 0,1,2,...n
   $$
   则$X$的数学期望存在,且
   $$
   EX = \sum\limits_{k=0}^nkC_n^kp^k(1-p)^{n-k} = \sum\limits_{k=1}^n\frac{k\cdot n!}{k!(n-k)!}p^k(1-p)^{n-k} \newline= np\sum\limits_{k=1}^n\frac{(n-1)!}{(k-1)!(n-1)!}p^{k-1}(1-p)^{(n-1)-(k-1)} = np
   $$

   

   $$
   D(x) = npq
   $$



3. 泊松分布$P(\lambda)$

   设随机变量$X$服从参数为$\lambda$的泊松分布,$\lambda>0$:

$$
P(X=k) = \frac{\lambda^k}{k!}e^{-\lambda},\quad k = 0,1...
$$
​	则$X$的数学期望存在,其值为
$$
EX = \sum\limits_{k=0}^{+\infty} kP(X=k) = \sum\limits_{k=1}^{+\infty}k\frac{\lambda^k}{k!}e^{-\lambda} = \lambda\sum\limits_{k=0}^{+\infty}\frac{\lambda^k}{k!}e^{-\lambda} = \lambda
$$


$$
D(X) = \lambda
$$



4. 均匀分布$U[a,b],-\infty < a< b < +\infty$
   $$
   EX =\frac{a+b}{2}
   $$

   $$
   D(X) = \frac{(b-a)^2}{12}
   $$

   

5. 指数分布$e(\lambda),\lambda>0$

   设随机变量$X$服从指数分布,其密度函数为
   $$
   p(x) = \lambda e^{-\lambda x},\quad x>0
   $$
   则$X$的数学期望存在,且
   $$
   EX = \int_{-\infty}^{+\infty}xp(x)dx = \frac{1}{\lambda}
   $$

   $$
   D(X) = \lambda^{-2}
   $$

6. $\Gamma$分布$G(\lambda,r),\lambda,r>0$

   设随机变量$X$服从参数为$\lambda ,r$的$\Gamma$分布,则其密度函数为
   $$
   p(x) = \frac{\lambda^r}{\Gamma(r)}x^{r-1}e^{-\lambda x},\quad x\geq 0
   $$
   $X$的数学期望存在,且
   $$
   EX = \int_{-\infty}^{+\infty}xp(x)dx=\int_0^{+\infty} \frac{x\lambda^r}{\Gamma(r)}x^{r-1}e^{-\lambda x}dx = \frac{r}{\lambda}\int_0^{+\infty}\frac{\lambda^{r+1}}{\Gamma(r+1)}x^re^{-\lambda x}dx = \frac{r}{\lambda}
   $$

   $$
   D(X) = r\lambda^{-2}
   $$

7. 正态分布$N(\mu,\sigma^2),\mu\in R,\sigma>0$
   $$
   EX = \mu
   $$

   $$
   D(X) = \sigma^2
   $$

   

### 随机变量函数的数学期望

+ 若随机变量$X$的函数$Y=g(x)$也是一个随机变量，设$g(x)$的数学期望存在，则下面的结论成立.

  - 若$X$为离散型随机变量,其分布率为$P(X=x_k)=p_k,\;k = 1,2,...$,则$Y$的数学期望为
    $$
    EY = E[g(x)] = \sum_{k=1}^{+\infty}g(x_k)P(X=x_k) = \sum_{k=1}^{+\infty}g(x_k)p_k
    $$

  - 若$X$为连续型随机变量，其密度为$p(x)$，则
    $$
    EY = E[g(x)] = \int_{-\infty}^{+\infty}g(x)p_xdx
    $$

+ 设随机向量$(X,Y)$的函数$Z=g(X,Y)$是一个随机变量，且期望存在，则下面的结论成立.

  - 若$(X,Y)$是一个离散型的随机向量，其联合分布率为$P(X=x_i,Y=y_j) = p_{ij},\;i,j = 1,2...$

    则
    $$
    EZ = E[g(X,Y)]=\sum\limits_{i = 1}^{+\infty}\sum\limits_{j=1}^{+\infty}g(x_i,y_j)p_{ij}
    $$

  - 若$(X,Y)$为连续型随机向量，其联合密度为$p(x,y)$，则
    $$
    EZ = E[g(X,Y)]=\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}g(x,y)p(x,y)dxdy
    $$

### 数学期望的性质

+ 对常数$a,b$,若$a\leq x\leq b$,则$a\leq EX\leq b$

+ 线性性质: 
  $$
  E(c_1X_1+c_2X_2+c_3X_3+...+c_nX_n) = c_1EX_1+c_2EX_2+...+c_nEX_n
  $$

+ 若$X,Y$相互独立,则$E[XY] = EX\cdot EY$.若$X_1,X_2,...,X_n$相互独立,则
  $$
  E(X_1X_2...X_n) = EX_1EX_2...EX_n
  $$

## 2. 方差与矩

+ 若$EX^2<+\infty$,则称$E(X-EX)^2$为随机变量$X$的方差,记为$D(X)$或$Var(X)$,即$D(X) = E(X-EX)^2$.而称$\sigma(X) = \sqrt{D(X)}$为$X$的均方差或标准差.
+ $D(X) = EX^2-(EX)^2$

### 常见分布的方差

+ 见常见分布的期望

### 方差的性质

+ 常数的方差为0;

+ 设$a,b$为任意有限常数,$X$的方差存在,则
  $$
  D(aX+b) =a^2D(X) 
  $$

+ 对任意方差存在的随机变量$X$和$Y$,其和或差的方差任然有限,且
  $$
  D(X\pm Y) = D(X)+D(Y)\pm2E[(X-EX)(Y-EY)]
  $$

+ (切尔雪夫不等式)设随机变量$X$的期望$EX$和方差$DX$均存在,则任意的$\epsilon>0$
  $$
  P(|X-EX|\geq\epsilon)\leq\frac{DX}{\epsilon^2}
  $$