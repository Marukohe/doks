---
title: "6 - 中间代码生成"
description: ""
lead: ""
date: 2021-04-09T18:23:18+08:00
lastmod: 2021-04-09T18:23:18+08:00
draft: false
images: []
menu: 
  docs:
    parent: "compiler"
weight: 5
toc: true
---

## 本章内容
+ 中间代码表示
  + 表达式的有向无环图DAG
  + 三地址代码：$x= y\ op\ z$
+ 类型检查
  + 类型、类型检查、表达式的翻译
+ 中间代码生成
  + 控制流、回镇

## 编译器前端的逻辑结构
+ 前端是对源语言进行分析并产生中间表示
+ 处理与源语言相关的细节，与目标机器无关
+ 前端后端分开的好处：不同的源语言、不同的机器可以得到不同的编译器组合
  {{< img src="6-1.png" alt="Rectangle" class="border-0" >}}

## 中间代码表示及其好处
+ 性质
  + 多种中间表示，不同层次
  + 抽象语法树
  + 三地址代码
+ 重定位
  + 为新的机器建编译器，只需要做从中间代码到新的目标代码的翻译器（前端独立）
+ 高层次的优化
  + 优化与源语言和目标机器都无关

## 中间代码的实现
+ 静态类型检查和中间代码生成的过程都可以用语法制导的翻译来描述和实现
+ 对于抽象语法树和这种中间表示的生成，第五章已经介绍过

## 生成抽象语法树的语法制导定义
+ $a+a*(b-c)+(b-c)*d$
  {{< img src="6-2.png" alt="Rectangle" class="border-0" >}}

## 表达式的有向无环图
+ 语法树中，公共子表达式每出现一次，就有一棵对应的子树
+ 表达式的有向无环图(Directed Acyclic Graph)能够指出表达式中的公共子表达式，更简洁地表示表达式
  {{< img src="6-3.png" alt="Rectangle" class="border-0" >}}

## DAG构造
+ 可以用和构造抽象语法树一样的SDD来构造
+ 不同的处理
  + 在函数Leaf和Node每次被调用时，构造新节点前先检查是否已存在同样的节点，如果已经存在，则返回这个已有的节点
+ 构造过程示例
  {{< img src="6-4.png" alt="Rectangle" class="border-0" >}}

## 三地址代码
+ 每条指定右侧最多有一个运算符
  + 一般情况可以写成$x=y\ op\ z$
+ 允许的运算分量
  + 名字：源程序中的名字作为三地址代码的地址
  + 常量：源程序中出现或生成的常量
  + 编译器生成的临时变量
+ 指定集合(1)
  + 运算/赋值指令：$x=y\ op\ z\quad x = op\ y$
  + 复制指令：$x = y$
  + 无条件转移指令：$goto\ L$
  + 条件转移指令：$if\ x\ goto\ L\quad if\ False\ x\ goto\ L$
  + 条件转移指令：$if\ x\ relop\ y\ goto\ L$
+ 指令集合(2)
  + 过程调用/返回
    + param $x_1$ //设置参数
    + param $x_2$
    + ...
    + param $x_n$
    + call p, n //调用子过程p,n为参数个数
  + 带下标的复制指令：$x=y[i],x[i] = y$
    + 注意：$i$表示离开数组位置第$i$个字节，而不是数组的第$i$个元素
  + 地址/指针赋值指令
    + $x=\\&y\quad x=*y\quad *x=y$

## 例子
+ 语句
  + $do\ i = i + 1;\ while(a[i]<v);$
  {{< img src="6-5.png" alt="Rectangle" class="border-0" >}}

## 三地址指令的四元式表示方法
+ 在实现时，可使用四元式/三元式/间接三元式/静态单赋值来表示三地址指令
+ 四元式：可以实现为记录（或结构）
  + 格式（字段）：$op\ arg_1\ arg_2\ result$
  + $op$：元素符的内部编码
  + $arg_1\ arg_2\ result$是地址
  + $x=y+z\quad\quad +\ y\ z\ x$
+ 单目运算符不使用$arg_2$
+ param运算不使用$arg_2$和$result$
+ 条件转移/非条件转移将目标标号放在result字段

## 四元式的例子
+ 赋值语句：$a = b*-c+b*-c$
  {{< img src="6-6.png" alt="Rectangle" class="border-0" >}}

## 三元式表示
+ 三元式(Triple)：$op\ arg_1\ arg_2$
+ 使用三元式的位置来引用三元式的运算结果
+ $x = y\ op\ z$需要拆分为（?是编号）
  - $?\ \ op\ y\ z$
  - $\quad = x\ (?)$
+ $x[i]=y$需要拆分为两个三元式
  + 求$x[i]$的地址，然后再赋值

## 三元式的例子
{{< img src="6-7.png" alt="Rectangle" class="border-0" >}}

## 间接三元式
+ 包含了一个指向三元式的指针的列表
+ 可以对这个列表进行操作，完成优化功能，操作时不需要修改三元式中的参数
{{< img src="6-8.png" alt="Rectangle" class="border-0" >}}

## 静态单赋值（SSA）
+ Static Single Assignment(SSA)中的所有赋值都是针对不同名的变量
+ 对于同一个变量在不同路径中定值的情况，可以使用$\phi$函数来合并不同的定值
  + if(flag) x = -1; else x = 1; y = x * a;
  + if(flag) x1 = -1; else x2 = 1; x3 = $\phi$(x1, x2), y = x3 * a;

## 类型和声明
+ 类型检查
  + 利用一组规则来检查运算分量的类型和运算符的预期类型是否匹配
+ 类型信息的用途
  + 差错、确定名字需要的内存空间、计算数组元素的地址、类型转换、选择正确的运算符
+ 本节的内容
  + 确定名字的类型
  + 变量的存储空间布局（相对地址)

## 类型表达式
+ 类型表达式：表示类型的结构体
  + 可能是基本类型
  + 也可能通过类型构造算子作用于类型表达式而得到
+ 如int[2][3]，表示由两层数组组成的数组
  + array(2,array(3,integer))
  + array是类型构造算子，有两个参数：数字和类型

## 类型表达式的定义
+ 基本类型（或类型名）是类型表达式
  + 如：boolean,char,integer,float,void...
+ 类型构造算子array作用于数字和类型表达式得到类型表达式，record作用于字段名和相应的类型得到类型表达式
+ 类型构造算子$\rightarrow$可得到函数类型的类型表达式
+ 如果s,t是类型表达式，其笛卡尔积$s\times t$也是类型表达式
  + 如struct{ int a[10]; float f; }st;
  + 对应于record((a$\times$ array(0..9,int))$\times$(f$\times$real))

## 类型等价
+ 不同的语言有不同的类型等价的定义
+ 结构等价：
  + 它们是相同的基本类型，或
  + 由相同的构造算子作用于结构等价的类型而得到，或
  + 一个类型是另一个类型表达式的名字
+ 名等价
  + 类型名仅代表自身（仅有前两个条件）

## 类型的声明
+ 处理基本类型、数组类型或记录类型的文法
  + $D\rightarrow T.id; D\mid\epsilon$
  + $T\rightarrow B\ C\mid record\ '\\{'D'\\}'$
  + $B\rightarrow int\mid float$
  + $C\rightarrow\epsilon\mid [num]\ C$
+ 应用该文法及其对应的语法制导定义，除了得到类型表达式之外，还得进行各种类型的存储布局

## 局部变量的存储布局
+ 变量的类型可以确定变量需要的内存
  + 即类型的宽度（该类型一个对象所需的存储单元的数量）
+ 函数的局部变量总是分配在连续的区间
  + 因此给每个变量分配一个相对于这个区间开始处的相对地址
+ 变量的类型信息保存在符号表中

## 计算$T$的类型和宽度的SDT
+ 综合属性：type,width
  + 全局变量$t$和$w$用于将类型和宽度信息从$B$传递到$C\rightarrow\epsilon$
  + 相当于$C$的继承属性（也可以把$t$和$w$替换为$C.t$和$C.w$）
  {{< img src="6-9.png" alt="Rectangle" class="border-0" >}}

## SDT运行的例子

## 声明序列的SDT(1)
+ 在处理一个过程/函数时，局部变量应该放到单独的符号表中
+ 这些变量的内存布局独立
  + 相对地址从0开始，变量的放置和声明的顺序相同
+ SDT的处理方法
  + 变量offset记录当前可用的相对地址
  + 每分配一个变量，offset增加相应的值（加宽度）
+ top.put(id.lexeme, T.type, offset)
  + 在符号表中创建条目，记录标识符的类型和偏移量

## 声明序列的SDT(2)
+ 可以把offset看作D的继承属性
  + D.offset表示D中第一个变量的相对地址
  + $P\rightarrow \\{\ D.offset = 0;\ \\}\ D$
  + $D\rightarrow T.id; \\{D_1.offset = D.offset + T.width; \\}\  D_1$
  {{< img src="6-10.png" alt="Rectangle" class="border-0" >}}

## 记录和类中的字段
+ 记录变量声明的翻译方案
+ 约定
  + 一个记录中各个字段的名字必须互不相同
  + 字段名的偏移量（相对地址），是相对于该记录的数据区字段而言的
+ 记录类型使用一个专用的符号表，对其各个字段的类型和相对地址进行单独编码
+ 记录类型record(t)：record是类型构造算子，t是符号表对象，保存该记录类型各个字段的信息
{{< img src="6-11.png" alt="Rectangle" class="border-0" >}}

## 表达式代码的SDD
+ 将表达式翻译成三地址代码的SDD
  + code表示代码
  + addr表示存放表达式结构的地址
  + new Temp()生成临时变量
  + gen()生成指令
  {{< img src="6-12.png" alt="Rectangle" class="border-0" >}}

## 增量式翻译方案
+ 类似于上一张中所述的边扫描边生成
+ gen不仅构造新的三地址指令，还要将它添加到至今为止已生成的指令序列之后
+ 不需要code指令保存已有的代码，而是对gen的连续调用生成一个指令序列
  {{< img src="6-13.png" alt="Rectangle" class="border-0" >}}

## 数组元素的寻址
+ 假设数组元素被存放在连续的存储空间中，元素从0到n-1编号，第i个元素的地址为
  + $base + i * w$
+ k维数组的寻址：假设数组按行存放，首先存放A[0][$i_2$]...[$i_k$]，然后存放A[1][$i_2$]...[$i_k$],...,那么A[$i_1$][$i_2$]...[$i_k$]的地址为
  + $base + i_1 * w_1+i_2 * w_2+...+i_k * w_k$
  + 其中$base,w$的值可以从符号表中找到

## 数组引用的翻译
+ 为数组引用生成代码要解决的主要问题
  + 数组引用的文法和地址计算相关联
+ 假定数组编号从0开始，基于宽度来计算相对地址
+ 数组引用相关文法
  + 非终结符号$L$生成数组名，加上一个下标表达式序列
    $$
    L\rightarrow L\ [E]\mid id\ [E]
    $$
  
## 数组引用生成代码的翻译方案(1)
+ 非终结符号$L$的三个综合属性
  + L.array是一个指向数组名字对应的符号表条目的指针(L.array.base为该数组的基地址)
  + L.addr指示一个临时变量，计算数组引用的偏移量
  + L.type是$L$生成的子数组类型
    + 对于任何（子）数组类型L.type，其宽度由L.type.width给出，L.type.elem给出其数组元素的类型

## 数组引用生成代码的翻译方案(2)
+ 核心是确定数组引用的地址
  {{< img src="6-14.png" alt="Rectangle" class="border-0" >}}

## 数组引用生成代码的翻译方案(3)
+ $L$的代码只计算了偏移量
+ 数组元素的存放地址应该根据偏移量进一步计算，即$L$的数组基址加上偏移量
+ 使用三地址指令$x=a[i]$
  {{< img src="6-15.png" alt="Rectangle" class="border-0" >}}

## 数组引用生成代码的翻译方案(4)
+ 使用三地址指令$a[i]=x$
  {{< img src="6-16.png" alt="Rectangle" class="border-0" >}}

## 类型检查和转换
+ 类型系统
  + 给每一个组成部分赋予一个类型表达式
  + 通过一组逻辑规则来表达类型表达式必须满足的条件
  + 可发现错误、提高代码效率、确定临时变量的大小
+ 类型检查可分为动态和静态两种
+ 强类型的：如果编译器中的类型系统能够保证它接受的程序在运行时刻不会发生类型错误，则该语言的这种实现称为强类型的

## 类型系统的分类
+ 类型综合
  + 根据子表达式的类型构造出表达式的类型
    if f的类型为s$\rightarrow$t且x的类型为s  
    then f(x)的类型为t
+ 类型推导
  + 根据语言结构的使用方式来确定该结构的类型
    if f(x)是一个表达式  
    them 对于某些类型$\alpha$和$\beta$，f的类型为$\alpha\rightarrow\beta$且x的类型为$\alpha$

## 函数/运算符的重载
+ 通过查看参数类解决函数重载的问题
+ $E\rightarrow f(E_1)$  
  {  
    if f.typeset = {$s_i\rightarrow t_i\mid 1\leq i\leq n$} and $E_1$.type=$s_k$  
    then E.type = $t_k$  
  }

## 类型转换
+ 假设在表达式$x * i$中，$x$为浮点数，$i$为整数，则结果应该是浮点数
  + $x$和$i$使用不能的二进制表示方式
  + 浮点数$*$和整数$*$使用不同的指令
  + $t_1=(float)i\ t_2=x\ fuml\ t_1$
+ 类型转换比较简单时的SDT
  $E\rightarrow E_1+E_2$  
  {  
    if($E_1.type=integer$ and $E_2.type=integer$) E.type = integer;  
    else if($E_1.type = float$ and $E_2.type=integer$) E.type = float;  
    ...  
  }

## 类型拓宽widening和窄化narrowing
+ Java的类型转换规则
+ 编译器自动完成的转换为隐式转换，程序员用代码指定的转换为显示转换
  {{< img src="6-17.png" alt="Rectangle" class="border-0" >}}

## 处理类型转换的SDT
+ 函数max求两个参数在拓宽层次结构中的最小公共祖先
+ 函数widen生成必要的类型转换代码
  {{< img src="6-8.png" alt="Rectangle" class="border-0" >}}
  {{< img src="6-9.png" alt="Rectangle" class="border-0" >}}

## 控制流的翻译
+ 布尔表达式可以用于改变控制流/计算逻辑值
+ 文法
  + $B\rightarrow B\lVert B\mid B\ \\&\\&\ B\mid\ !B\mid (B)\mid E\ rel\ E\mid \boldsymbol{true}\mid \boldsymbol{false}$
+ 语义
  + $B_1\lVert B_2$中$B_1$为真时，不计算$B_2$，整个表达式为真，因此，当$B_1$为真时应该跳过$B_2$的代码
  + $B_1\ \\&\\&\ B_2$中$B_1$为假时，不计算$B_2$，整个表达式为假
+ 短路代码
  + 通过跳转指令实现控制流，逻辑运算符本身不出现

## 控制流语句的翻译
+ 控制流语句
  + $S\rightarrow if(B)\ S_1$
  + $S\rightarrow if(B)\ S_1\ else\ S-2$
  + $S\rightarrow while(B)\ S_1$
+ 继承属性
  + $B.true$：B为真时的跳转目标
  + $B.false$：B为假时的跳转目标
  + $S.next$：S执行完毕时的跳转目标

## 语法制导的定义(1)
{{< img src="6-20.png" alt="Rectangle" class="border-0" >}}

## 语法制导的定义(2)
{{< img src="6-21.png" alt="Rectangle" class="border-0" >}}
+ 增量式生成代码
  $$
  S\rightarrow while(\newline
    \\{
      begin = newlable();\ B.true = newlabel();\ B.false=S.next;\ gen(begin':');
    \\}B\newline
  )\\{S_1.next=begin;\ gen(B.true':';)\\}\ S_1\ \\{gen('goto'begin);\\}
  $$

## 布尔表达式的控制流翻译
+ 生成的代码执行时跳转到两个标号之一
  + 表达式的值为真时，跳转到$B.true$
  + 表达式的值为假时，跳转到$B.false$
+ $B.true$和$B.false$是两个继承属性，根据B所在的上下文指向不同的位置
  + 如果B是if语句条件表达式，分别指向then和else分支；如果没有else分支，则B.false指向if语句的下一条指令
  + 如果B是while语句的条件表达式，分别指向循环体的开头和循环出口处

## 布尔表达式的代码的SDD
{{< img src="6-22.png" alt="Rectangle" class="border-0" >}}
{{< img src="6-23.png" alt="Rectangle" class="border-0" >}}

## 布尔值和跳转代码
+ 程序中出现布尔表达式也可能是求值：$x=a<b$
+ 处理方法：
  + 建议表达式的语法树，根据表达式的不同角色来处理
+ 文法
  + $S\rightarrow id=E;\mid if(E)\ S\ \mid while(E)\ S\ \mid S\ S$
  + $E\rightarrow E || E\mid E \\&\\& E\mid\ !E\mid E\ rel\ E\mid ...$
+ 根据E的语法树节点所在的位置
  + $S\rightarrow while(E)\ S_1$中的$E$，生成跳转代码
  + 对于$S\rightarrow id=E$，生成计算右值代码

## 回填(1)
+ 为布尔表达式和控制流语句生成目标代码
  + 关键问题：某些跳转指令应该跳转到哪里？
+ 例如：$if(B)\ S$
  + 按照短路代码的翻译方法，$B$的代码中有一些跳转指令在$B$为假时执行
  + 这些跳转指令的目标应该跳过$S$对应的代码，但生成这些指令时，$S$的代码尚未生成，因此目标不确定
  + 通过语句的继承属性$next$来传递，需要第二趟处理

## 回填(2)
+ 基本思想：
  + 记录$B$中跳转指令如$goto\ S.next$的标号，但不生成跳转目标，这些标号被记录到$B$的综合属性$B.falselist$中
  + 当$S.next$的值成为已知时（即$S$的代码生成完毕时），把$B.falselist$中的所有指令的目标都填上这个值
+ 回填技术
  + 生成跳转指令时不指定跳转目标，而是使用列表记录这些不完整指令的标号
  + 当知道正确的跳转目标时再填写目标
  + 列表中的每一个指令都指向同一个目标

## 布尔表达式的回填翻译(1)
+ 布尔表达式在取值true/false时分别跳转到某目标
+ 综合属性
  + truelist：包含跳转指令标号的列表，这些指令在取值true时执行
  + falselist：包含跳转指令标号的列表，这些指令在取值false时执行
+ 辅助函数
  + makelist(i)：创建一个包含跳转指令标号$i$的列表
  + merge($p_1,p_2$)：将$p_1$和$p_2$指向的标号列表合并然后返回
  + backpatch(p,i)：将$i$作为跳转目标插入$p$的所有指令中

## 布尔表达式的回填翻译(2)
+ 文法中引入非终结符$M$
+ 在适当的时候获取将要生成指令的标号
{{< img src="6-24.png" alt="Rectangle" class="border-0" >}}

## 控制转移语句的回填(1)
+ 语句
  + $S\rightarrow if(B)\ S\mid if(B)\ S\ else\ S\mid while(B)\ S\mid \\{L\\}\mid A$
  + $L\rightarrow L\ S\mid S$
+ 综合属性nextlist
  + nextlist中跳转指令的目标是$S$执行完毕后紧接着执行的下一条指令的标号
  + 考虑$S$是if语句，while语句的子语句时，分别应该跳转到哪里？

## 控制转移语句的回填(2)
+ M：用M.instr记录下一条指令的标号
+ N：生成goto指令坯，N.nextlist包含该指令标号
{{< img src="6-25.png" alt="Rectangle" class="border-0" >}}
{{< img src="6-26.png" alt="Rectangle" class="border-0" >}}

## Break、Continue的处理
+ 虽然break、continue在语法上是一个独立的子句，但是它的代码和外围语句相关
+ 方法：(break语句)
  + 跟踪外围循环语句$S$
  + 生成一个跳转指令坯
  + 将这个指令坯的位置加入到S的nextlist中
