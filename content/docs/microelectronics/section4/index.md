---
title: "半导体三极管"
description: ""
lead: ""
date: 2021-04-09T23:56:25+08:00
lastmod: 2021-04-09T23:56:25+08:00
draft: false
images: []
menu: 
  docs:
    parent: "microelectronics"
weight: 18
toc: true
---

## 303.半导体三极管

### 三极管结构和符号

{{< img src="三极管结构和符号.png" alt="Rectangle" class="border=0" >}}

+ 结构特点：
  + 发射区的掺杂浓度最高；
  + 集电区掺杂浓度低于发射区，且面积大；
  + 基区很薄，一般在几个微米，且掺杂浓度最低。

### 放大状态下三极管的工作原理

+ 三极管的放大作用是在一定外部条件控制下，通过载流子传输体现出来的。
+ **实现放大的外部条件：发射结正偏，集电结反偏**

1. BJT内部载流子的传播过程  
   (1). 发射区向基区扩散载流子
   $$
   I_E=I_{EN}+I_{EP}\approx I_{EN}
   $$
   (2). 载流子在基区扩散与复合
   $$
   I_B=I_{EP}+I_{BN}-I_{CBO}
   =I_{EP}+I_{EN}-I_{CN}-I_{CBO}
   =I_E-I_C
   $$
   (3). 集电区收集载流子
   $$
   I_C=I_{CN}+I_{CBO}
   $$
2. BJT的电流分配关系
   根据传输过程
   $$
   I_E\approx I_{EN}
   $$
   $$
   I_C=I_{CN}+I_{CBO}
   $$
   $$
   I_B\approx I_E-I_C
   $$
   共基极直流放大系数
   $$
   \bar{\alpha}=\frac{I_{CN}}{I_E}=\frac{I_C-I_{CBO}}{I_E}\approx\frac{I_C}{I_E}
   $$
   $\alpha$为电流放大系数。它只与管子的结构尺寸和掺杂浓度有关，与外加电压无关，一般$\alpha=0.9\sim 0.99$。
   共射极直流放大系数
   $$
   \bar{\beta}=\frac{\bar{\alpha}}{1-\bar{\alpha}}
   $$
   $$
   I_{CEO}=(1+\bar{\beta})I_{CBO}
   $$
   则，$I_C=\bar{\beta}I_B+I_{CEO}$  
   当$I_C\gg I_{CEO}$时，忽略$I_{CEO}$，则$\bar{\beta}\approx\frac{I_C}{I_B}$  
   $\bar{\beta}$是共射极直流放大系数，同样，他也只与管子的结构尺寸和掺杂浓度有关，与外加电压无关。一般$\bar{\beta}\gg 1$。

### 三极管的三种组态

{{< img src="三极管三种组态.png" alt="Rectangle" class="border=0" >}}

+ 三极管的放大作用，主要是依靠它的发射极电流能够通过基极传输，然后到达集电极实现的。
+ **实现这一传输过程的两个条件**：
  1. 内部条件：发射区杂质浓度远大于基区杂质浓度，且基区很薄；
     （由于基区很薄，掺杂浓度又低，因此电子与空穴复合机会少，大多数电子能从基区扩散到集电结边界）
  2. 外部条件：发射结正向偏置，集电结反向偏置。

### BJT的$I-V$特性曲线

### 1. 共射极连接时的$V-I$特性曲线

1. 输入特性曲线
   $$
   i_B=f(v_{BE})\mid_{v_{CE}=const}
   $$
   当$v_{CE}=0V$时，相当于发射结的正向伏安特性曲线  
   当$v_{CE}\geq 1V$时，$v_{CB}=v_{CE}-v_{BE}>0$，集电结已进入反偏状态，收集载流子能力增强，基区复合减少，同样的$v_{BE}$下$I_B$减小，特性曲线右移。  
   {{< img src="特性曲线.png" alt="Rectangle" class="border=0" >}}
   输入特性曲线的三个部分：
   1.死区 2. 非线性区 3. 近似线性区
   {{< img src="输入特性曲线三个部分.png" alt="Rectangle" class="border=0" >}}
2. 输出特性曲线
   $$
   i_C=f(v_{CE})\mid_{i_B=const}
   $$
   输出特性曲线的三个区域：
   + 饱和区：$i_C$明显受$v_{CE}$控制的区域，该区域内，一般$v_{CE}<0.7V$（硅管）。此时，发射结正偏，集电结正偏或反偏电压很小。
   + 截止区：$i_C$接近零的区域，相当于$i_B=0$的曲线的下方。此时，$v_{BE}$小于死区电压。
   + 放大区：$i_C$平行于$v_{CE}$轴的区域，曲线基本平行等距。此时，发射结正偏，集电结反偏。

### BJT的主要参数

1. 电流放大系数  
   共发射极直流电流放大系数$\bar{\beta}$
   $$
   \bar{\beta}=\frac{I_C-I_{CEO}}{I_B}\approx\frac{I_C}{I_B}\mid v_{CE}=const
   $$
   共发射极交流电流放大系数
   $$
   \beta=\Delta I_C/\Delta I_B\mid v_{CE}=const
   $$
   共基极直流电流放大系数$\bar{\alpha}$
   $$
   \bar{\alpha}=(I_C-I_{CBO})/I_E\approx I_C/I_E
   $$
   共基极交流电流放大系数
   $$
   \alpha = \Delta I_C/\Delta I_E\mid V_{CB}=const
   $$
2. 极间反向电流
   + 集电极基极间反向饱和电流$I_{CBO}$
     发射极开路时，集电结的反向饱和电流
   + 集电极发射极间的反向饱和电流$I_{CEO}$
     $$
     I_{CEO}=(1+\bar{\beta})I_{CBO}
     $$
     即输出特性曲线$I_B=0$的那条曲线所对应的$Y$坐标的数值。$I_{CEO}$也称为集电极发射极间穿透电流。
3. 极限参数：
   + 集电极允许最大电流$I_{CM}$
   + 集电极最大允许功率损耗$P_{CM}$
     $$
     P_{CM}=I_CV_{CE}
     $$
   + 反向击穿电压
     + $V_{(BE)CBO}$——发射极开路时的集电结反向击穿电压
     + $V_{(BR)EBO}$——发射极开路时发射结的反向击穿电压
     + $V_{(BR)CEO}$——基极开路时集电极和发射极间的击穿电压。
   {{< img src="主要参数.png" alt="Rectangle" class="border=0" >}}

### 温度对BJT参数及特性的影响

1. 温度对BJT参数的影响
   + 温度对$I_{CBO}$的影响  
   + 温度每升高$10^\circ C$，$I_{CBO}$约增加一倍。
   + 温度对$\beta$的影响  
     温度每升高$1^\circ C$，$\beta$的值约增大$0.5\% \sim 1\%$。
   + 温度对反向击穿电压$V_{(BR)CBO}、V_{(BR)CEO}$的影响  
     温度升高时，$V_{(BR)CBO}、V_{(BR)CEO}$都会有所提高
2. 温度对BJT特性曲线的影响
   {{< img src="温度影响.png" alt="Rectangle" class="border=0" >}}

## 304.三极管放大电路

### 基本共射极放大电路的组成

{{< img src="共射极放大.png" alt="Rectangle" class="border=0" >}}

### 共射极放大电路的工作原理

1. 静态：输入信号$v_i=0$时，方法电路的工作状态称为静态或直流工作状态。
   静态工作点：$Q(I_{BQ}，I_{CQ}，V_{CEQ})$
   $$
   I_{BQ}=\frac{V_{BB}-B_{BEQ}}{R_b}
   $$
   $$
   I_{CQ}=\beta I_{BQ}+I_{CEQ}\approx \beta I_{BQ}
   $$
   $$
   V_{CEQ}=V_{CC}-I_{CQ}R_c
   $$
2. 动态：输入正弦信号$v_s$后，电路将处在动态动作情况。此时，BJT各极电流及电压都将在静态值得基础上随输入信号作相应的变化。

### BJT放大电路的图解分析法

1. 静态工作点的图解分析  
   采用该方法分析静态工作点，必须已知三极管的输入输出特性曲线
   + 列输入回路方程
     $$
     v_{BE}=V_{BB}-i_{B}R_b
     $$
   + 列输出回路方程（直流负载线）
     $$
     v_{CE}=V_{CC}-i_CR_c
     $$
   + 在输入特性曲线上，作出直线$v_{BE}=V_{BB}-I_BR_b$，两线的交点即是$Q$点，得到$I_{BQ}$。
   + 在输出特性曲线上，作出直线$v_{CE}=V_{CC}-I_CR_c$，与$I_{BQ}$曲线的交点即为$Q$点，从而得到$V_{CEQ}$和$I_{CQ}$。
   {{< img src="静态工作点.png" alt="Rectangle" class="border=0" >}}
2. 动态工作情况的图解分析
   + 根据$v_s$的波形，在$BJT$的输入特性曲线上画出$V_{BE}，I_B$的波形；
   + 根据$i_B$的变化范围在输出特性曲线图上画出$i_C$和$v_{CE}$的波形
   {{< img src="动态工作情况.png" alt="Rectangle" class="border=0" >}}
3. 静态工作点对波形失真的影响  
   静态工作点太高容易出现饱和失真，**所以要选择一个合适的静态工作点**
   {{< img src="饱和失真.png" alt="Rectangle" class="border=0" >}}
   静态工作点太低会导致截至失真；  
   工作点$Q$要设置在输出特性曲线放大区的中间部位，要有合适的交流负载线。  
   信号幅度不大，不产生失真和保证一定的电压增益，$Q$可选得低些，可以省电，因为$I_CR_C$消耗的功率少了。
4. 电阻变化对负载线的影响
   {{< img src="电阻变化.png" alt="Rectangle" class="border=0" >}}
   + (1). 负载线斜率变小，$Q$点左移
   + (2). $I_B$变小，导致$I_C$变小，$Q$点右下移
   + (3). 负载线左移，$I_B，I_C$变小，$Q$点左下移
   + 没有影响，不变
5. 温度对工作点的影响
   {{< img src="温度变化.png" alt="Rectangle" class="border=0" >}}

### 基极分压式射极偏置电路

+ 通过选择一个合适的$R_{b1}，R_{b2}$的阻值，能满足$I_1\gg I_{BQ}$，使$I_2\approx I_1$，可认为基极直流电位基本上为一固定值，即
  $$
  V_{VQ}=\frac{R_{b2}V_{CC}}{R_{b1}+R_{b2}}
  $$
+ {{< img src="基极分压.png" alt="Rectangle" class="border=0" >}}
+ $R_e$取值越大，反馈作用越强，一般取$I_1=(5\sim 10)I_{BQ}$，$V_B=3\sim 5V$
