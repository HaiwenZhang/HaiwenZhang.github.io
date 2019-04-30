---
title: Scala函数式编程札记01
date: 2017-12-25 16:48:17
categories:
- Scala
tags:
- Scala
- 函数式编程
comments: true
---

最近为了研究Spark，开启了Scala的学习之旅，然后便一发不可收拾。。。。。。那么Scala是一门什么样的语言呢？有些人说它是JVM上的C++；也有些人说它是Haskell，不过比Haskell弱。Scala的官网上有一句话：Object-Oriented Meets Functional，什么意思呢？简单来说就是“面向对象”这位帅气的小伙子偶遇了“函数式”这位邻家小妹妹，发生了一段计算机界惊天动地的情缘，于是便产生了Scala。一说到了面向对象，C++、Java、Python等便在脑海里显现出来了，可是对于函数式编程很多工程师肯定是只听其声不见其人。那么今天就随我一起去揭开邻家小妹妹神秘的面纱吧！

<!--more-->

## 1. 什么是函数式编程？
函数式编程基于一个及其简单的前提，那就是它只是用纯函数编程。说道这有人就会想你这不扯淡吗？加个纯字就可以装神秘了吗！那我还纯净水呢！其实纯函数不是我们通常所使用的函数，而是一种没有副作用的函数。问题又来了，什么是没有副作用？这问题嘛好难回答，还好数学上有一种叫做反证的方法，我们先来看看副作用的常见例子，看完我想你就能大致知道什么是没有副作用了。
1. 修改变量
2. 修改数据结构
3. 在对象上设置一个属性
4. 抛出异常或者因为一个错误而停止程序
5. 在终端中打印数据或者读入用户的输入
6. 文件I/O
7. 屏幕上显示

看完这几点是不是想惊呼实际编写代码不都跟这些打交道，用了函数式以后还怎么写程序了？说好的努力写代码，做一个备受欢迎的App，然后升职加薪迎娶白富美，走向人生巅峰的呢。不玩了不玩了，伦家还是回去写面向对象的代码吧！不要急不要急，妹纸嘛！千呼万唤始出来，心急是吃不了热豆腐，这么轻易的放弃了以后还想不想脱单了(说的好像我脱单似的o(╥﹏╥)o 。咳咳咳，扯远了扯远了，来来让我们上一波热腾腾的代码，尝点鲜。

## 2. 函数式编程实例
将一个旧的List中的每个元素都乘以3得到一个新的List，我想聪明的你一定很快想到下面的解决方案。(伦家不会Scala咋办？出门左转跳楼就会了[（＊￣（エ）￣）](https://goo.gl/ZzJkJB)

```Scala
import scala.collection.mutable.ListBuffer

object Demo extends App {

  val x = List(10,20,30,40)
  val mutable = new ListBuffer[Int]
  for (e <- x) {
    mutable += (e * 3)
  }
  println(mutable.toList)
}

```

上面的方案就是传统的命令式编程，那么函数式编程会怎么做呢？

```Scala

object Demo extends App {
  val x = List(10,20,30,40)
  val y = x.map( i => i* 3)

  println(y)
}

```

代码是不是很简洁、干净。作为一名有志青年，是不是在想如果自己的每一行代码都这么简洁，那么分分钟装逼这种事情还不是顺手的事情。O(∩_∩)O哈哈~既然我们有相同的想法，那就跟着我一起走向Scala的函数式编程的世界。