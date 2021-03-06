---
title: "4 - 语法分析"
description: ""
lead: ""
date: 2021-04-09T18:03:52+08:00
lastmod: 2021-04-09T18:03:52+08:00
draft: false
images: []
menu: 
  docs:
    parent: "compiler"
weight: 3
toc: true
---

## 语法分析器

### 程序设计语言构造的描述

+ 程序设计语言构造的语法可使用 **上下文无关文法(CFG)** 或 **BNF** 来描述

### 语法分析器的作用

+ 从词法分析器获得词法单元的序列，确认该序列是否可以由语言的文法生成
+ 对于语法错误的程序，报告错误信息
+ 对于语法正确的程序，生成语法分析树

### 语法分析器的分类

+ 通用语法分析器
  + 可以对任意文法进行语法分析
  + 效率很低，不适合用于编译器
+ 自顶向下语法分析器（通常用于处理LL文法）
  + 从语法分析树的根部开始构造语法分析树
+ 自底向上语法分析器（通常用于处理LR文法）
  + 从语法分析树的叶子开始构造语法分析树
+ 后两种方法
  + 总是从左到右、追个扫描词法单元
  + 只能处理特定类型的文法，但足以描述程序设计语言

## 上下文无关文法

+ 一个 **上下文无关文法(CFG)** 包含四个部分
  + **终结符号**：组成串的基本符号（词法单元名字）
  + **非终结符号**：表示串的集合的语法变量
  + **开始符号**：某个被指定的非终结符号
    + 它对应的串的集合就是文法的语言
  + **产生式**：描述将终结符号和非终结符号组成串的方法

### 推导（1）

+ 推导：
  + 将待处理的串中的某个非终结符号替换为这个非终结符号的某个产生式的体
  + 从开始符号出发，不断进行上面的替换，就可以得到文法的不同句型
+ 例子：
  + 文法：$E\rightarrow -E|\ E+E\ |E\* E\ |\ (E)\ |\ id$
  + 推导序列：$E\Rightarrow -E\Rightarrow -(E)\Rightarrow -(id)$

### 推导（2）

+ 推导的正式定义
  + 如果$A\rightarrow\gamma$是一个产生式，那么$\alpha A\beta\Rightarrow \alpha\gamma\beta$
  + **最左（右）推导**：$\alpha(\beta)$中不包含非终结符号
    + 符号：$\underset{lm}{\stackrel{\*}{\Rightarrow}},\ \underset{rm}{\stackrel{\*}{\Rightarrow}}$
+ 经过零步或者多步推导出：$\stackrel{\*}{\Rightarrow}$
+ 经过一步或者多步推导出：$\stackrel{+}{\Rightarrow}$

### 句型/句子/语言

+ 句型
  + 如果$S\stackrel{\*}{\Rightarrow}\alpha$，那么$\alpha$就是文法$S$的句型
  + 可能既包含非终结符号，又包含终结符号，也可以是空串
+ 句子
  + 文法的句子就是不包含非终结符号的句型
+ 语言
  + 文法$G$的语言就是$G$的句子的集合，记为$L(G)$
  + $w$在$L(G)$中当且仅当$w$是$G$的句子，即$S\stackrel{\*}{\Rightarrow} w$

### 语法分析树

+ 推导的图形表示形式
  + 根节点的标号是文法的开始符号
  + 每个叶子结点的标号是非终结符号，终结符号或者$\epsilon$
  + 每个内部结点的标号是非终结符号
  + 每个内部结点表示某个产生式的一次应用
    + 结点的标号为产生式头，其子结点从左到右是产生式的体
+ 树的叶子组成的序列是根的文法符号的一个句型
+ 一棵语法分析书可对应多个推导序列
  + 但只有 **唯一的最左推导及最右推导**

### 二义性（1）

+ 二义性：如果一个文法可以为某个句子生成多棵语法分析树，这个文法就是二义的

### 二义性（2）

+ 程序设计语言的文法通常是无二义性的，否则就会导致一个程序有多种“正确”的解释
+ 有些二义性的情况可以方便文法或语法分析器的设计，但需要消二义性规则来剔除不要的语法分析树

### 上下文无关文法和正则表达式

+ 上下文无关文法比正则表达式的能力更强

### 文法及其生成的语言

+ 语言是从文法的开始符号出发，能推导得到的所有句子的集合
+ 如何验证文法$G$所确定的语言$L$：
  + 证明$G$生成的每个串都在$L$中
  + 证明$L$中的每个串都能被$G$生成

### 设计文法（1）

+ 文法能够描述程序设计语言的大部分用法
  + 但不是全部，比如，标识符的先声明后使用则无法用上下文无关文法描述
  + 因此语法分析器接受的语言是程序设计语言的超集；必须通过语义分析来剔除一些符号文法，但不合法的程序

### 设计文法（2）

+ 在进行高效的语法分析之前，需要对文法做以下处理
  + 消除 **二义性**：
    + 二义性：文法可以为一个句子生成多棵不同的分析树
  + 消除 **左递归**：
    + 左递归：文法中的一个非终结符号$A$使得对某个串$\alpha$，存在一个推导$A\stackrel{+}{\Rightarrow}A\alpha$，则称这个文法是左递归的
  + 提取 **左公因子**

### 二义性的消除

+ 一些二义性文法可被改为等价的无二义性的文法
+ 二义性的消除方法没有规律可循

### 左递归的消除

+ 左递归的定义：如果一个文法中有非终结符号$A$使得$A\stackrel{+}{\Rightarrow}A\alpha$，那么这个文法就是左递归的
+ 立即左递归（规则左递归）：文法中存在一个形如$A\rightarrow A\alpha$的产生式
+ 自顶向下的语法分析技术不能处理左递归的情况，因此需要消除左递归，但是自底向上的技术可以处理左递归

### 立即左递归的消除

+ 假设非终结符号$A$存在立即左递归的情形
  $$
  A\rightarrow A\alpha_1\mid A\alpha_2\mid ...\mid A\alpha_m \mid \beta_1\mid...\mid \beta_n
  $$
+ 可替换为
  $$
  A\rightarrow \beta_1 A'\mid ...\mid \beta_nA'\newline
  A'\rightarrow\alpha_1A'\mid...\mid \alpha_mA'\mid \epsilon
  $$

### 通用的左递归消除方法

+ 输入：没有环和$\epsilon$产生式的文法$G$
+ 输出：等价的无左递归的文法
+ 步骤：
  {{< img src="4-1.png" alt="Rectangle" class="border-0" >}}

### 预测分析法简介

+ 试图从开始符号推导出输入符号串
+ 每次为最左边的非终结符号选择适当的产生式
  + 通过查看下一个输入符号来选择这个产生式
  + 有多个可能的产生式时则无能为力

### 提取公因子的文法变换

+ 算法
  + 输入：文法$G$
  + 输出：等价的提取了左公因子的文法
  + 方法：对于每个非终结符号$A$，找出它的两个或者多个可选产生式之间最长公共前缀
    + $A\rightarrow\alpha\beta_1\mid ...\mid \alpha\beta_n\mid\gamma$
    + $A\rightarrow\alpha A'\mid \gamma,\ A'\rightarrow\beta_1\mid ...\mid\beta_n$

## 语法分析技术

### 自顶向下的语法分析

+ 为输入串构造语法分析树
  + 从分析树的根节点开始，按照先根次序，深度优先地创建各个结点
  + 对应于最左推导
+ 基本步骤
  + 确定对句型中最左边的非终结符号应用哪个产生式
  + 然后对该产生式与输出符号进行匹配
+ 关键问题
  + 确定对最左边的非终结符号应用哪个产生式

### 递归下降的语法分析

+ 每个非终结符号对应于一个过程，该过程负责扫描此非终结符号对应的结构
+ 程序执行从开始符号对应的过程开始
  + 当扫描输入整个串时宣布分析成功完成
  {{< img src="4-2.png" alt="Rectangle" class="border-0" >}}

### 递归下降分析技术的回溯

+ 原算法第一行的语句中需要保存当前的扫描指针，在报错之后将指针移动到下一个扫描点，所有产生式都报错，程序才能报告错误

### FIRST和FOLLOW

+ 在自顶向下的分析技术中，使用向前看几个符号来确定产生式（通常是看一个符号）
+ 当前句型是$xA\beta$，而输入是$xa...$，那么选择产生式$A\rightarrow\alpha$的必要条件是下列之一
  + $\alpha\stackrel{\*}{\Rightarrow}a$
  + $\alpha\stackrel{\*}{\Rightarrow}\epsilon$，且$\beta$以$a$开头，即在某个句型中$a$跟在$A$之后
  + 如果按照这两个条件选择时能够保证 **唯一性**，那么我们就可以避免回溯
+ 因此，我们定义FIRST和FOLLOW

### FIRST

+ $FIRST(\alpha)$
  + 可以从$\alpha$推导得到串的首符号的集合
  + 如果$\alpha\stackrel{\*}{\Rightarrow}\epsilon$，那么$\epsilon$也在$FIRST(\alpha)$中
+ FIRST函数的意义
  + $A$的产生式$A\rightarrow\alpha\mid\beta$，且$FIRST(\alpha)$和$FIRST(\beta)$不想交
  + 下一个输入符号是$a$，若$a\in FIRST(\alpha)$，则选择$A\rightarrow\alpha$，若$a\in FIRST(\beta)$，则选择$A\rightarrow\beta$

### FIRST的计算方法

+ $\sout{偷懒截图了}$
  {{< img src="4-3.png" alt="Rectangle" class="border-0" >}}

### FOLLOW

+ $FOLLOW(A)$
  + 可能在某些句型中紧跟在$A$右边的非终结符号的集合
+ $FOLLOW(A)$的意义
  + 如果$A\rightarrow\alpha$，当$\alpha\rightarrow\epsilon$或$\alpha\Rightarrow\epsilon$，$FOLLOW(A)$可以帮助我们选择恰当的产生式
  + 例如：$A\rightarrow\alpha$，而$b\in FOLLOW(A)$，如果$\alpha\Rightarrow\epsilon$，而当前的输入符号是$b$，则可以选择$A\rightarrow\alpha$，因为$A$最终到达了$\epsilon$，而且后面跟着$b$

### FOLLOW的计算方法

+ 算法
  {{< img src="4-4.png" alt="Rectangle" class="border-0" >}}

### LL(1)文法（1）

+ 其中的1表示只需要往前看1一个符号就能决定用最左推导的哪个产生式展开
+ 定义：对文法的任意连个产生式$A\rightarrow\alpha\mid\beta$
  + 不存在终结符号$a$使得$\alpha$和$\beta$都可以推导出$a$开头的串
  + $\alpha$和$\beta$最多只有一个可推导出空串
  + 如果$\beta$可推导出空串，那么$\alpha$不能推导出以$FOLLOW(A)$中任何终结符号开头的串
+ 等价于
  + $FIRST(\alpha)\cap FIRST(\beta)=\emptyset$
  + 如果$\epsilon\in FIRST(\beta)$，那么$FIRST(\alpha)\cap FOLLOW(A)=\emptyset$

### 预测分析表构造算法

+ 输入：文法$G$
+ 输出：预测分析表$M$
+ 方法：
  + 对于文法$G$的每个产生式$A\rightarrow\alpha$
    + 对于$FIRST(\alpha)$中的每个终结符号$a$，将$A\rightarrow\alpha$加入到$M[A,a]$中
    + 如果$\epsilon$在$FIRST(\alpha)$中，那么对于$FOLLOW(A)$中的每个符号$b$，将$A\rightarrow\alpha$也加入到$M[A,b]$中
  + 最后在所有的空白条目中填入$\textcolor{red}{error}$

### LL(1)文法的递归下降分析

+ 递归下降语法分析程序由一组过程组成
+ 每个非终结符号对应于一个过程，该过程负责扫描该非终结符号对应的数据结构
+ 可以使用当前的输入符号来唯一地选择产生式
+ 算法的过程用了预测分析表，如果当前需要选择$A$的产生式，且输入符号为$a$，则选择$M[A,a]$中的产生式

### 非递归的预测分析(1)

+ 初始化时，栈中仅包含开始符号$S$(和\$)
+ 如果栈顶的元素是终结符号，那么进行匹配
+ 如果栈顶元素是非终结符号
  + 使用预测分析表来选择适当的产生式
  + 在栈顶用产生式右部替换产生式左部

### 预测分析算法

{{< img src="4-5.png" alt="Rectangle" class="border-0" >}}

### 自底向上的语法分析

+ 为一个输入串构造语法分析树的过程
+ 从叶子（输入串中的终结符号，将位于分析树的底端）开始，向上到达根结点
  + 在实际的语法分析过程中并不一定会构造出相应的分析树，但是用分析树的概念可以方便理解
+ 重要的自底向上玉峰分析的通用框架：移入——归约
+ 简单LR技术(SLR)，LR技术(LR)

### 归约

+ 自底向上的语法分析过程可以看成是从串$w$归约为文法开始符号$S$的过程
+ 归约步骤
  + 一个与某产生式体相匹配的特定子串被替换为该产生式头部的非终结符号

### 句柄

+ 对输入从左到右扫描，并进行自底向上的语法分析，实际可以反向构造出一个最右推导
+ 句柄：
  + 最右句型中和某个产生式体相匹配的子串，对它的归约代表了该最右句型的最右推导的最后一步
  + 正式定义：如果$A\underset{rm}{\stackrel{\*}{\Rightarrow}}\alpha Aw\underset{rm}{\stackrel{\*}{\Rightarrow}}\alpha\beta w$，那么紧跟在$\alpha$之后的$\beta$是$A\rightarrow\beta$的一个句柄
+ 在一个最右句型中，句柄右边只有终结符号
+ 如果文法是无二义性的，那么每个句型有且只有一个句柄

### 移入—归约分析技术

+ 使用一个栈来保存归约/扫描移入的文法符号
+ 栈中符号（从底向上）和待扫描的符号组成了一个最右句型
+ 开始时刻：栈中只包含\$，而输入为w\\$(公式渲染有问题，多了一个反斜杠)
+ 结束时刻：栈中为S\$，而输入为\\$
+ 在分析过程中，不断移入符号，并在识别到句柄时进行归约
+ 句柄被识别时总是出现在栈的顶部

### 主要分析动作

+ 移入：将下一个输入符号移入到栈顶
+ 归约：将句柄归约为相应的非终结符号
  + 句柄总是在栈顶
  + 具体操作时弹出句柄，压入被归约到的非终结符号
+ 接受：宣布分析过程成功完成
+ 报错：发现语法错误，调用错误恢复子程序

### 移入-归约分析中的冲突

+ 对于有些不能使用移入-归约分析的文法，不管什么样的移入-归约分析器都会到达无法判断是否该移入还是归约。

### LR语法分析技术

+ LR(k)的语法分析概念
  + L表示最左扫描，R表示反向构造出最右推导
  + k表示最多向前看$k$个符号
+ 当$k$增大时，相应的语法分析器的规模急剧增大
  + $k=2$时，程序语言的语法分析器的规模通常非常庞大
  + 当$k=0,1$时，已经可以解决很多语法分析的问题，因此具有实践意义

### LR语法分析器的优点

+ 由表格驱动，虽然手工构造表格工作量很大，但表格可以自动生成
+ 对于几乎所有的程序设计语言，只要写出上下文无关语法，就能构造出识别该语言的LR语法分析器
+ 最通用的无回溯移入-归约分析技术
+ 能分析的文法比LL(k)文法更多

### LL(0)项

+ 项：文法的一个产生式加上在其中某处的一个点
  + $A\rightarrow\cdot XYZ,A\rightarrow X\cdot YZ,A\rightarrow XY\cdot Z,A\rightarrow XYZ\cdot$
  + 注意：$A\rightarrow\epsilon$只对应一个项$A\rightarrow\cdot$
+ 直观含义
  + 项$A\rightarrow\alpha\cdot\beta$表示已经扫描/归约到了$\alpha$，并期望接下来的输入中经过扫描/归约得到$\beta$，然后把$\alpha\beta$归约到$A$
  + 如果$\beta$为空，表示我们可以把$\alpha$归约到$A$

### LL(0)项集规范族的构造

+ 三个相关定义
  + 增广文法
  + 项集闭包CLOSURE
  + GOTO函数

#### 增广文法

+ $G$的增光文法$G'$实在$G$中增加新的开始符号$S'$，并加入产生式$S'\rightarrow S$而得到的
+ 显然$G'$和$G$接受相同的语言，且按照$S'\rightarrow S$进行归约实际上就表示已经将输入符号串归约成开始符号

#### 项集闭包CLOSURE

+ 项集闭包：如果$I$是文法$G$的一个项集，$CLOSURE(I)$就是根据下列两条规则从$I$构造得到的项集
  + 将$I$中的各项加入到$CLOSURE(I)$中
  + 如果$A\rightarrow \alpha\cdot B\beta$在$CLOSURE(I)$中，而$B\rightarrow\beta$是一个产生式，且项$B\rightarrow\cdot\beta$不在$CLOSURE(I)$中，就将该项加入其中，不断应用该规则直到没有新项可加入
+ 意义：$A\rightarrow\alpha\cdot B\beta$，表示希望看到由$B\beta$推导出的串，那要先看到由$B$推导出的串，因此加上$B$的各个产生式对应的项
+ 算法
  {{< img src="4-6.png" alt="Rectangle" class="border-0" >}}

#### GOTO函数

+ $I$是一个项集，$X$是一个文法符号，则GOTO($I,X$)定义为$I$中所有包含形如项$[A\rightarrow\alpha\cdot X\beta]$所对应的项$[A\rightarrow \alpha X\cdot\beta]$的集合的闭包

### 求LR(0)项集规范族的算法

+ 从初始项集开始，不断计算各种可能的后继，直到生成所有项集
  {{< img src="4-7.png" alt="Rectangle" class="border-0" >}}

### LR(0)自动机的构造

+ 构造方法
  + 基于规范LR(0)项集族可以构造LR(0)自动机
  + 规范LR(0)项集族中的每个项集对应于LR(0)自动机的一个状态
  + 状态转换：如果GOTO(I,X)=J，则从I到J有一个标号为X的转换
  + 开始状态位CLOSURE(\{S'$\rightarrow$S\})对应的项集

### LR(0)自动机的作用

+ 假设文法符号串$\gamma$使LR(0)自动机从开始状态运行到状态（项集）$j$
+ 如果$j$中存在项$A\rightarrow\alpha\cdot$，那么
  + 在$\gamma$之后添加一些终结符号可以得到最后一个最右句型
  + $\alpha$和$\gamma$的后缀，且是该句型的句柄（对应于产生式$A\rightarrow\alpha$）
  + 表示可能找到了当前最右句型的句柄，可以归约
+ 如果$j$中存在项$B\rightarrow\alpha\cdot X\beta$，那么
  + 在$\gamma$之后添加$X\beta$和一些终结符号可以得到一个最右句型
  + 该句型中$\alpha X\beta$是句柄，但还没有找到，需要移入
+ LR(0)自动机的使用
  + 移入-归约时，LR(0)自动机被用于识别句型
  + 已得到的文法符号序列对应于LR(0)自动机的一条路径
+ 无需每次用该文法符号序列来运行LR(0)自动机
  + 文法符号可省略，由LR(0)状态可确定相应的文法符号
  + 在移入后，根据原来的栈顶状态可以知道新的状态
  + 在归约时，根据归约产生式的右部长度弹出相应状态，也可以根据此时的栈顶状态知道新的状态

### LR语法分析器的结构

+ 所有的分析器都使用相同的驱动程序
+ 分析表随文法以及LR分析技术的不同而不同
+ 栈中存放的是状态序列，可求出相应的符号序列
+ 分析程序根据栈顶和当前输入，通过分析表确定下一步动作

### LR语法分析表的结构

+ 两个部分：动作ACTION、转换GOTO
+ ACTION表项有两个参数：状态$i$，终结符号$a$
  + 移入$j$：$j$是新状态，把$j$压入栈（同时移入$a$）
  + 归约$A\rightarrow\beta$：把栈顶的$\beta$归约为$A$（并根据GOTO表项压入新状态）
  + 接受：接受输入，完成分析
  + 报错：在输入中发现语法错误
+ GOTO表项
  + 如果GOTO[$I_i,A$]=$I_j$，那么GOTO[$i,A$]=$j$

### LR语法分析器的格局

+ LR语法分析器的格局包括了栈中内容和余下输入$(s_0s_1...s_m,a_ia_{i+1}...a_n\\$)$
  + 第一个分量是栈中的内容（右侧是栈顶）
  + 第二个分量是余下的输入
+ LR语法分析器的每一个状态都对应一个文法符号（$s_0$除外）
  + 令$X_i$为$s_i$对应的符号，那么$X_1X_2...X_ma_i...a_n$对应于一个最右句型

### LR语法分析器行为

+ 对于格局$(s_0s_1...s_m,a_i...a_n\\$)$，LR语法分析器查询条目ACTION$[s_m,a_i]$确定相应的动作
  + 移入$s$：执行移入动作，将状态$s$（对应于输入$a_i$）移入到栈中，得到新格局$(s_0s_1...s_{m+1},a_{i+1}...a_n\\$)$
  + 归约$A\rightarrow\beta$：将栈顶的$\beta$归约到$A$，压入状态$s$，得到新格局$(s_0s_1...s_{m-r}s,a_ia_{i+1}...s_n\\$)$，其中$r$是$\beta$的长度，状态$s=\mathrm{GOTO}[s_{m-r},A]$
  + 接受：语法分析过程完成
  + 报错：发现语法错误，调用错误恢复例程

### LR语法分析算法

+ 输入：文法$G$的LR语法分析表，输入串$w$
+ 输出：如果$w$在$L(G)$中，则输出自底向上语法分析过程中的归约步骤，否则输出错误提示
+ 算法如下：
  {{< img src="4-8.png" alt="Rectangle" class="border-0" >}}

### SLR语法分析表的构造

+ 以LR(0)自动机为基础的SLR语法分析表构造算法
  + 构造增广文法$G'$的LR(0)项集规范族$\\{I_0,I_1,...,I_n\\}$
  + 状态$i$对应项集$I_i$，相关的ACTION/GOTO表条目如下
    + $[A\rightarrow\alpha\cdot a\beta]$在$I_i$中，且$\mathrm{GOTO}(I_i,a)=I_j$，则$\mathrm{ACTION}[i,a]$=“移入$j$”
    + $[A\rightarrow\alpha\cdot]$在$I_i$中，那么$\mathrm{FOLLOW}(A)$中所有$a$，$\mathrm{ACTION}[i,a]$=“按$A\rightarrow\alpha$归约”
    + 如果$[S'\rightarrow S\cdot]$在$I_i$中，那么将$\mathrm{ACTION}[i,\\$]$设为“接受”
    + 如果$\mathrm{GOTO}[I_i,A]=I_j$，那么在GOTO表中，$\mathrm{GOTO}[i,A]=j$
  + 空白条目设为"error"
+ 如果SLR分析表没有冲突，该文法就是SLR的
+ 思想：把$\alpha$归约称为$A$，后面需要是$\mathrm{FOLLOE}(A)$中的终结符号，否则只能移入

### 非SLR(1)文法的例子

+ 文法：
  + $S\rightarrow L=R\mid R$
  + $S\rightarrow *R\mid id$
  + $R\rightarrow L$
+ 对于项集$I_2$，输入$=$既能根据第一个项移入，也能根据第二个项归约
  {{< img src="4-9.png" alt="Rectangle" class="border-0" >}}

### SLR语法分析器的弱点

+ SLR技术解决冲突的方法
  + 项集中包含$[A\rightarrow\alpha\cdot]$时，按照$A\rightarrow\alpha$进行归约的条件是下一个输入符号$a$可以在某个句型中跟在$A$之后
  + 假设此时栈中的符号为$\beta\alpha$
    + 如果$\beta A\alpha$不是任何最右句型的前缀，那么即使$\alpha$在某个句型中跟在$A$之后，仍不应该按照$A\rightarrow\alpha$归约
    + 进行归约的条件更加严格可以降低冲突的可能性
+ $[A\rightarrow\alpha\cdot]$出现在项集中的条件
  + 首先$[A\rightarrow\cdot\alpha]$出现在某个项集中，然后逐步读入/归约到$\alpha$中的符号，点不断后移，到达末端
  + 而$[A\rightarrow\cdot\alpha]$出现的条件是$B\rightarrow\beta\cdot A\gamma$出现在项中
  + 期望首先按照$A\rightarrow\alpha$归约，然后将$B\rightarrow\beta\cdot A\beta$中的点移到$A$之后
  + 显然，在按照$A\rightarrow\alpha$归约时要求下一个输入符号是$\gamma$的第一个符号，但是从LR(0)项集中不能确定这个信息

### 更强大的LR语法分析器

+ 规范LR方法（LR方法）
  + 添加项$[A\rightarrow\cdot\alpha]$时，把期望的向前看符号也加入项中（成为LR(1)项集）
  + 这个做法可以充分利用向前看符号，但是状态很多
+ 向前看LR（LALR文法）
  + 基于LR(0)项集族，但每个LR(0)项都带有向前看符号
  + 分析能力强于SLR文法，且分析表和SLR分析表一样大
  + LALR已经可以处理大部分的程序设计语言

### LR(1)项

+ LR(1)项中包含更多信息来消除一些归约动作
+ 实际的做法相当于“分裂”一些LR(0)状态，精确指明何时应该归约
+ LR(1)项的形式$[A\rightarrow\alpha\cdot\beta, a]$
+ $a$称为向前看符号，可以是终结符号或者\$
+ $a$表示如果将来要按照$A\rightarrow\alpha\beta$进行归约，归约时的下一个输入符号必须是$a$
+ 当$\beta$非空时，移入动作不考虑$a$，$a$传递到下一状态

### LR(1)项和可行前缀

+ 如果$[A\rightarrow\alpha\cdot B\beta,a]$对于前缀$\gamma$有效，那么
  + $[B\rightarrow\cdot\theta,b]$对于$\gamma$有效的条件是$b=\mathrm{FIRST}(\beta a)$
  + $S\underset{rm}{\stackrel{\*}{\Rightarrow}}\delta Aw\underset{rm}{\Rightarrow}\delta\alpha B\beta w\underset{rm}{\stackrel{\*}{\Rightarrow}}\delta\alpha Bxw\underset{rm}{\Rightarrow}\delta\alpha\theta xw$
  + $b$应该是$xw$的第一个符号，或$xw$为空且b=\$
  + 如果$x$为空，则$b=a$
+ 如果$[A\rightarrow\alpha\cdot X\beta,a]$对前缀$\gamma$有效，那么
  + $[A\rightarrow\alpha X\cdot\beta,a]$对$\gamma X$有效

### 构造LR(1)项集

+ LR(1)项集族的构造与LR(0)项集族类似，但是CLOSURE和GOTO有所不同
  + 在CLOSURE中，当由项$[A\rightarrow\alpha\cdot B\beta,a]$生成新项$[B\rightarrow\cdot\theta,b]$时，$b$必须在$\mathrm{FIRST}(\beta a)$中
  + 对LR(1)项集中的任意项$[A\rightarrow\alpha\cdot B\beta,a]$，总有：$a$在$\mathrm{FOLLOW}(A)$中
    + 初始项满足这个条件
    + 每次求CLOSURE项集时，新产生的项也满足这个条件

### LR(1)项集的CLOSURE算法

+ 注意在添加$[B\rightarrow\cdot\gamma,b]$时，$b$的取值范围
  {{< img src="4-10.png" alt="Rectangle" class="border-0" >}}

### LR(1)项集的GOTO算法

+ 和LR(0)项集的GOTO算法基本相同
  {{< img src="4-11.png" alt="Rectangle" class="border-0" >}}

### LR(1)项集族的构造算法

+ 和LR(0)项集族的构造算法相同
  {{< img src="4-12.png" alt="Rectangle" class="border-0" >}}

### LR(1)语法分析表的构造

+ 步骤
  + 构造得到LR(1)项集族$C'=\\{I_0,I_1,...,I_n\\}$
  + 状态$i$对应于项集$I_i$，其分析动作如下
    + $[A\rightarrow\alpha\cdot a\beta, b]$在项集中，且GOTO($I_i,a$)=$I_j$，那么ACTION$[i,a]$=“移入$j$”
    + $[A\rightarrow\alpha\cdot, a]$在项集中，那么ACTION$[i,a]$=“按照$A\rightarrow \alpha$归约”
    + $[S'\rightarrow S\cdot,\\$]$在项集中，那么ACTION$[i,\\$]$=“接受”
  + GOTO表项：GOTO[$i,A$]=$j$，如果GOTO$(I_i,A)=I_j$
  + 没有填写的条目为error
  + 如果条目有冲突，则不是LR(1)的
  + 初始状态对应于$[S'\rightarrow \cdot S,\\$]$所在的项集

### 构造LALR语法分析表

+ SLR(1)语法分析表的分析能力较弱
+ LR(1)语法分析表的状态数量很大
+ LALR(1)是实践中常用的方法
  + 状态数量和SLR(1)的状态数量相同
  + 能够方便地处理大部分常见程序设计语言的构造

### LALR分析技术的基本思想

+ 寻找具有相同核心的LR(1)项集，并把它们合并成为一个项集
  + 项集的核心就是项的第一分量的集合
+ 一个LR(1)项集的核心是一个LR(0)项集
+ GOTO($I,X$)的核心只由$I$的核心决定，因此被合并项集的GOTO目标也可以合并

### 合并引起的冲突

+ 原来无冲突的LR(1)分析表在合并之后得到LALR(1)分析表，新表中可能存在冲突
+ 合并不会导致移入/归约冲突
  + 假设合并之后在$a$上存在移入/归约冲突，即存在项$[B\rightarrow\beta\cdot a\beta,?]$和$[A\rightarrow \alpha\cdot, a]$
  + 因为被合并的项集具有相同的核心，因此被合并的所有项集中都包括$[B\rightarrow\beta\cdot a\beta,?]$，而$[A\rightarrow \alpha\cdot, a]$也必然在某个项集中，那么这个项集必然已经存在冲突
+ 合并会引起归约/归约冲突，即不能确定按照哪个产生式进行归约

### LALR分析表构造算法

+ 步骤
  + 构造得到LR(1)项集族$C=\\{I_0,I_1,...,I_n\\}$
  + 对于LR(1)中的每个核心，找出所有具有该核心的项集，并把这些项集替换为它们的并集
  + 令$C'=\\{J_0,J_1,...,J_m\\}$为得到的LR(1)项集族
    + 按照LR分析表的构造方法得到ACTION表（如果存在冲突，则这个文法不是LALR的）
  + GOTO表的构造：设$J$是一个或多个LR(1)项集的并集，令$K$是所有和GOTO$(I,X)$具有相同核心的性阿基的并集，那么GOTO$(J,X)=K$
+ 得到的分析表称为LALR语法分析表

### LALR分析器和LR分析器

+ 处理语法正确的输入时，LALR语法分析器和LR语法分析器的动作序列完全相同
  + 栈中的状态名字不同，但是状态序列之间有对应关系
  + 如果LR分析器压入状态$I$，那么LALR分析器压入$I$对应的合并项集
+ 当处理错误的输入时，LALR可能多执行一些归约动作，但不会多移入一个符号

### LALR技术本质

+ 对LR(1)项集规范族中的同核心项集进行合并
  + 是的分析表保持了LR(1)项中的向前看符号信息
  + 又使状态数减少到与SLR分析表的一样多

### 二义性文法的使用

+ 二义性文法都不是LR的
+ 某些二义性文法是有用的
  + 可以简洁地描述某些结构
  + 隔离某些语法结构，对其进行特殊处理
+ 对某些二义性文法
  + 可以通过消除二义性规则来保证每个句子只有一棵语法分析树
  + 可以在LR分析器中实现这个规则

### 优先级/结合性消除冲突

+ 二义性文法
  $$
  E\rightarrow E\ +\ E\mid E\ *\ E\mid (E)\mid id
  $$
+ 等价于
  $$
  E\rightarrow E\ +\ T\mid T\quad T\rightarrow T\ *\ F\mid F\quad F\rightarrow (E)\mid id
  $$
+ 二义性文法的优点
  + 容易修改运算符的优先级和结合性
  + 简洁：如果有多个优先级，那么无二义性文法将引入太多的非终结符号
  + 高效：不需要处理$E\rightarrow T$这样的归约

### 二义性表达式文法的LR(0)项集

+ 文法
  $$
  E\rightarrow E\ +\ E\mid E\ *\ E\mid (E)\mid id
  $$
+ 冲突：
  + $I_7,I_8$中有冲突，且不可能通过向前看符号解决
{{< img src="4-16.png" alt="Rectangle" class="border-0" >}}

### 冲突的原因以及解决

+ 当栈顶状态为7时，表明
  + 栈中状态序列对应的文法符号序列为：$...E+E$
  + 如果下一个符号为+或*，移入还是归约
+ 如果*的优先级大于+，且+是左结合的
  + 下一个符号为*时，我们应该移入\*
  + 下一个符号为+时，我们应该将$E+E$归约为$E$
  
### 解决冲突之后的SLR(1)分析表

+ 对于状态7
  + +时归约
  + *时移入
+ 对于状态8
  + 总执行归约
+ 分析表
  {{< img src="4-17.png" alt="Rectangle" class="border-0" >}}

### 语法错误的处理

+ 错误难以避免，编译器需要有处理错误的能力
+ 程序中可能存在不同层次的错误
  + 词法错误、语法错误、语义错误、逻辑错误
+ 语法分析器中错误处理程序的设计目标
  + 清晰准确地报告出现地错误，并指出错误的位置
  + 能从当前错误中恢复，以继续检测后面地错误

### 预测分析中的错误恢复

+ 错误恢复：
  + 当预测分析器报错时，表示出入的串不是句子
  + 使用者希望预测分析其能进行错误处理后继续语法分析过程，以便在一次分析中找到更多的语法错误
  + 可能恢复得并不成功，之后找到的语法错误是假的
+ 两类错误恢复方法：
  + 恐慌模式、短语层次的恢复

### 恐慌模式

+ 基本思想
  + 语法分析器忽略输入中的一些符号，直到出现由设计者选定的某个同步词法单元
  + 解释：
    + 语法分析过程总是试图在输入前缀中找到和某个非终结符号对应的串，出现错误表明已经不可能找到对应这个非终结符号（程序结构）的串了
    + 如果编程错误仅局限于这个程序结构，我们可考虑跳过该程序结构，假装已经找到了它，然后继续进行语法分析处理
  + **同步词法单元**就是这个程序结构结束的标志

### 同步词法单元的确定

+ 文法符号$A$的同步集合的启发式规则：
  + 非终结符号：
    + 将$\mathrm{FOLLOW}(A)$中所有符号放入$A$的同步集合中
    + 将高层次非终结符号对应串的开始符号加入到较低层次非终结符号的同步集合
      + 比如：语句的开始符号，if/while等可作为表达式的同步集合
    + 将$\mathrm{FIRST}(A)$中的符号加入到$A$的同步集合
      + 碰到这些符号时，可能意味着前面的符号是多余的符号
  + 终结符号：在栈顶的终结符号出现匹配错误时，可直接弹出该符号，并且发出消息称已经插入了这个终结符号

### 恐慌模式的例子

+ 对$E,T,F$的表达式进行语法分析时，可直接使用其$\mathrm{FOLLOW}$值作为同步集合
  + synch表示一直忽略到同步集合，然后弹出非终结符号
  + 空条目表示忽略输入符号（多）
  + 终结符号不匹配时，弹出栈中终结符号（漏）
  {{< img src="4-13.png" alt="Rectangle" class="border-0" >}}

### 短语层次的恢复

+ 在预测语法分析表的空条目中插入错误处理例程的函数指针
  + 例程可以改变、插入或删除输入中的符号，并发出适当的错误消息
  
### LR语法分析中的错误恢复

+ 查询ACTION表时可能发现报错条目
  + 假设栈中的符号串为$\alpha$，当前输入符号为$a$，报错表示不可能存在终结符号串$x$使得$\alpha ax$是一个最右句型
+ 恐慌模式的错误恢复策略
  + 从栈顶向下扫描，找到状态$s$，$s$有一个对应于某个非终结符号$A$的GOTO目标（$s$之上的状态被丢弃）
  + 在输入中丢弃一些符号，直到一个可以跟在$A$之后的符号$b$（不丢弃$b$），并将$\mathrel{GOTO}(s,A)$压栈，继续进行分析
  + 基本思想：假定当前试图归约到$A$但碰到了语法错误，因此设法扫描完包含语法错误的$A$的子串，假装找到了一个$A$的实例
+ 短语层次的恢复
  + 检查LR分析表中的每个报错条目，根据语言的特性来确定程序员最可能犯了什么错误，然后构造适当的恢复程序

## 语法分析器生成工具

### 语法分析器生成工具YACC/Bison

+ YACC的使用方法如下
  {{< img src="4-14.png" alt="Rectangle" class="border-0" >}}

### YACC源程序的结构

+ 声明
  + 放置C声明和对词法单元的声明
+ 翻译规则
  + 指明产生式及相关的语义动作
+ 辅助性C语言历程
  + 被直接拷贝到生成的C语言源程序中
  + 可在语义动作中使用
  + 包括yylex()，这个函数返回词法单元，可以由Lex生成
  
### 翻译规则的格式

+ 说明
  + 第一个产生式的头被看作开始符号
  + 语义动作是C语句序列
  + `$$`表示和产生式相关的属性值，`$i`表示产生式体中第$i$个文法符号的属性值
  + 在按照某个产生式规约时，执行相应的语义动作，可以根据\$i来计算\\$\\$的值
  {{< img src="4-15.png" alt="Rectangle" class="border-0" >}}

### YACC中的冲突处理

+ 缺省处理方法
  + 归约/移入冲突：总是移入（悬空else的解决）
  + 归约/归约冲突：选择列在前面的产生式
+ 通过确定终结符号的优先级/结合性来解决冲突
  + 结合性：%left,%right,%nonassoc
  + 移入$a$/按$A\rightarrow a$归约：比较$a$和$A\rightarrow a$的优先级再选择
    + 终结符号的优先级按在声明部分出现的顺序而定
    + 产生式的优先级设为它最右的终结符号的优先级，也可以加标记%prec<终结符号>，指明产生式的优先级等同于该终结符号
  
### YACC的错误恢复

+ 使用错误产生式来完成语法错误恢复
  + 错误产生式$A\rightarrow error\ \alpha$
  + 例如：$stmt\rightarrow error;$
+ 定义哪些非终结符号有错误恢复动作
  + 比如：表达式、语句、块、函数定义等非终结符号
+ 当语法分析器遇到错误时
  + 不断弹出栈中状态，直到栈顶状态包含项$A\rightarrow\cdot error\ \alpha$
  + 分析器将$error$移入栈中
  + 如果$\alpha$为空，分析器直接执行归约，并调用相关的语义动作；否则跳过一些符号，找到可以归约为$\alpha$的串为止
