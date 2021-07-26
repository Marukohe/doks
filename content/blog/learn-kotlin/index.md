---
title: "Learn Kotlin"
description: ""
lead: ""
date: 2021-04-07T09:15:58+08:00
lastmod: 2021-04-07T09:15:58+08:00
draft: false
weight: 50
images: [""]
contributors: [He Wei]
---

## 变量、常量和类型

+ 变量定义格式：`变量定义关键字 变量名 类型定义 赋值运算符 赋值`  

  ```kotlin
  var experinencePoints : Int = 5
  ```

  `var`用于定义可变变量，`val`可用于定义只读变量，Kotlin支持类型推断，有时可不用添加类型定义。

## 条件语句

+ 条件表达式：Kotlin中可用if/else语句判断得到结果赋值给变量

  ```kotlin
  var checkSomething = true
  var a = if(checkSomething) 1 else 2
  ```

+ when表达式：有时可用when表达式代替if/else表达式，when表达式代码更简洁
  
  ```kotlin
  var healthPoints = 89
  val healthStatus = when (healthPoints) {
      100 -> "is in excellent condition!"
      in 90..99 -> "has a few scratches."
      in 75..89 -> if (isBlessed) {
          "has some minor wounds but is healing quite quickly!"
      } else {
          "has some minor wounds."
      }
      else -> "is in awful condiciton!"
  }
  ```

## 函数

+ 函数结构：函数头+函数体

  ```kotlin
  private fun formatHealthStatus(healthPoints: Int, isBlssed: Boolean): String {
      ...
      return healthStatus
  }
  ```

+ 单表达式函数：使用赋值运算符=后面跟表达式

  ```kotlin
  private fun castFireball(numFireballs: Int = 2) = 
    println("A glass of Fireball springs into existence. (x$numFireballs)")
  ```

+ Unit函数：表示没有返回值的函数
  
## 匿名函数与函数类型

+ 定义一个匿名函数

  ```kotlin
  fun main(args: Array<String>) {
      println({
          val currentYear = 2021
          "Welcome to SimVillate, Mayor! (copyright $currentYear)"
      }())
  }
  ```

  `()`表示调用匿名函数

+ 匿名函数也有类型，类型为`函数类型(function type)`
+ 把匿名函数赋值给变量

  ```kotlin
  val greetingFunction: () -> String = {
      val currentYear = 2021
      "Welcome to SimVillate, Mayor! (copyright $currentYear)"
  }
  ```

  匿名函数隐式返回函数体最后一条语句的结果

+ 函数参数

  ```kotlin
  val greetingFunction: (String) -> String = { playerName ->
      val currentYear = 2021
      "Welcome to SimVillate, $playerName! (copyright $currentYear)"
  }
  println(greetingFunction("Guyal"))
  ```

  当函数只有一个参数时，可以使用`it`关键字指代这个参数，不用写`playerName`

+ 类型推断：Kotlin的类型推断也支持函数类型

  ```kotlin
  val greetingFunction = { playerName: String ->
      val currentYear = 2021
      "Welcome to SimVillate, $playerName! (copyright $currentYear)"
  }
  ```

+ 定义参数是函数的函数

  ```kotlin
  val greetingFunction = { playerName: String, numBuildings: Int ->
      val currentYear = 2021
      println("Adding $numBuildings houses")
      "Welcome to SimVillate, $playerName! (copyright $currentYear)"
  }
  fun runSimulation(playerName: String, greetingFunction: (String, Int) -> String) {
      val numBuildings = (1..3).shuffled.last()
      println(greetingFunction(playerName, numBuildings))
  }

  runSimulation("Guyal", greetingFuntion)
  ```

+ 函数引用

  ```kotlin
  inline fun numCount(
      numA: Int, 
      numB: Int,
      numCalculator: (Int, Int) -> Int) {
    println(numCalculator(numA, numB))
  }

  fun addNum(a: Int, b: Int) {
      return a + b
  }

  numCount(::addNum)
  ```

+ 函数类型可以作为另一个函数的返回类型
+ 在Kotlin中，匿名函数能修改并引用定义在自己的作用域之外的变量。这表明，匿名函数饮用者定义自身的函数里的变量。

## 字符串

+ 解构：这个特性允许在一个表达式里给多个变量赋值，可以简化变量赋值。
  
  ```kotlin
  menuData = "shandy,Dragon's Breath,5.91"
  val (type, name, price) = menuData.split(',')
  ```
