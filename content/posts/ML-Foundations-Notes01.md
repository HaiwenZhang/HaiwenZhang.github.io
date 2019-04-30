---
title: 机器学习基石笔记：学习问题
date: 2018-06-10 20:54:45
categories:
- Machine Learning
tags:
- 机器学习基石
comments: true
markup: mmark
---
## 一、What is Machine Learning

首先解释一下什么是“学习”？对于学习一词我们并不陌生，从来到这个世界的第一天起我们就开始用我们好奇的眼睛和智慧的大脑去观察这个世界、理解世界，在不断的练习和积累之后掌握了某种技能，这便是“学习”。同样了我们希望机器也可以像人类一样，通过观察大量的数据训练，发现事物规律，获得某种分析问题、解决问题的能力，这就是“机器学习”。

<!--more-->

![](/images/machine_learning_foundations/learing_problem_01.png)

机器学习可以被定义为：Improving some performance measure with experence computed from data. 也就是机器从数据中学习和总结经验，从数据中找出某种规律或者模型，并用它来解决实际问题。

## 二、Why use Machine Leanring

现实生活中很多问题不能用简单的编程规则解决，而且我们也很难定义问题的解决方案。但是从机器学习的定义中可以看出机器学习并没有根据一条条规则进行判别，而是从数据中观察和识别，学习到某些可以用来解决问题的方案。而且一旦机器学习学到了解决问题的能力，那么它能够比人很快的做出决定，以及在大规模应用中使用。借用一句名言“Give a computer a fish, you feed it for a day. Teach it how to fish, you feed it for a lifetime.” 因此机器学习要做的事情就是教机器捕鱼。

于是我们便可知在什么情况下需要使用机器学习？
1. 事物本身存在某种潜在规律
2. 某些问题难以使用普通编程解决
3. 有可以用于学习潜在规律的数据

## 三、Applications of Machine Learning

从衣食住行到教育，娱乐等各个方面机器学习都有广泛的应用。比如说我们可以从Twitter的推文数据中发现餐厅的食物是否好吃；购物网站的向我们推荐我们可能喜欢的商品；Netflix会根据用户的浏览记录和观看记录，向用户推荐电影。。。。。。

## 四、Components of Learning

### 1、基本数学符号

1. 输入：$x \in X$
2. 输出：$y \in Y$
3. 目标函数：$f: X \to Y$ (学不到)
4. 训练数据：$D = \{(x_{1}, y_{1}), (x_{2}, y_{2}), ... ,(x_{n}, y_{n})\}$
5. Hypothesis: $g: X \to Y$ (一个机器学习模型对应了很多不同的hypothesis，通过演算法$A$，选择一个最佳的hypothesis对应的函数称为$g$，$g$能最好地表示事物的内在规律，也是我们最终想要得到的模型表达式。)

### 2、机器学习学习的流程

![](/images/machine_learning_foundations/learing_problem_02.png)

对于目标函数$f$，我们是未知的同时在实际中几乎不可能得到的。但是我们手上有一些训练数据$D$,假设是监督式学习，其中有输入$x$，也有输出$y$。机器学习的过程，就是根据先验知识选择模型，该模型对应的Hypothesis set（用$H$表示），$H$中包含了许多不同的Hypothesis，通过演算法$A$，在训练数据$D$上进行训练，选择出一个最好的hypothes，对应的函数表达式$g$就是我们最终要求的。($g$最接近目标函数$f$)