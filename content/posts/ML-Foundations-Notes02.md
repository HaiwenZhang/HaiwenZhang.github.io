---
title: 机器学习基石笔记：感知机
date: 2018-06-14 16:10:05
categories:
- Machine Learning
tags:
- 机器学习基石
comments: true
markup: mmark
---

## 一、Perceptron Hypothesis Set

假设:
 
特征空间：$x \subseteq R^n$

输出空间：$y=\{+1, -1\}$

于是关于感知机有：

$$
h(x) = sign(\sum_{i=1}^nw_ix_i - threshold)
$$

<!--more-->

为了计算方便，通常我们把阈值threshold当成$w_0$, 引入一个$x_0=-1$与$w_0$相乘，于是$h(x)$的表达式可以做如下变换：

$$
\begin{aligned}
   h(x) &= sign(\sum_{i=1}^nw_ix_i - threshold) \\
   &= sign(\sum_{i=1}^nw_ix_i + (threshold)\cdot(-1)) \\
   &= sign(\sum_{i=1}^nw_ix_i + w_0\cdot(-1)) \\
   &= sign(\sum_{i=0}^nw_ix_i) \\
   &= sign(W^TX)
\end{aligned}
$$

假设Perceptrons在二维平面上，即$h(x)=sign(w_0x_0 + w_1x_1 + w_2x_2)$.
其中$w_0x_0 + w_1x_1 + w_2x_2=0$是平面上一条分类直线，直线一边是正类(+1,用蓝色的$\circ$表示),另一边则是负类(-1, 用红色的x表示).不同的$w$对应二维平面上不同的直线，下图就是两个感知机模型的二维平面图：

![](/images/machine_learning_foundations/perceptron_01.png)

注意一下，感知机线性分类不限于二维空间，在高维的空间中，我们将不同类分开的“线”称之为线性超平面。

## 二、Perceptron Learning Algorithm(PLA)

现在我们有了第一个机器学习算法——感知机，接下来我们就要设计一个机器学习演算法$A$，去学到一个最好的直线使得该直线可以将所有的正类和负类分开。那么怎么去设计这样一个演算法呢？

先列出我们现有的条件：
1. 我们想学到一个最好的$g \approx f$ ($f$ 未知);
2. 在数据集$D$中，至少存在一个$g(x_n)=f(x_n)=y_n$；
3. 困难得地方是$H$有无限种可能。

Idea:
* 从某一个$g_0$开始，不断地修正$g$在数据集$D$上所犯的错误；
* $g_0$用$w_0$表示。

算法：

![](/images/machine_learning_foundations/perceptron_02.png)

## 三、Guarantee of PLA

根据PLA的定义，当找到一条直线，能将所有平面上的点都分类正确，那么PLA就停止了。要达到这个终止条件，就必须保证$D$是线性可分。如果是非线性可分的，那么，PLA就不会停止。那么对于线性可分的情况下，我们只是直观的觉得PLA会停止，接下来我们尝试去证明一下这个结论：

我们不妨假设存在这样一条直线可以将类分开，令这条直线的权重为$w_f$，则对于数据集$D$上任意一个点满足

$$
y_n=sign(w_{f}^Tx_{n})
$$

于是有一下条件成立：
* 对于任意一点成立，$w_f$有：

$$
y_{n(t)}w_{f}x_{n(t)} \geq \min\limits_{n}y_{n}w_{f}x_{n} > 0
$$ 

* $w_{t}$在有错的时候更新，所以一定有：

$$
  sign(w_{t}^Tx_{n(t)})	\neq
   y_{n(t)} \Leftrightarrow y_{n(t)w_{t}^Tx_{n(t)}} \le 0
$$

* $w_{t}$的更新方式为：

$$
w_{t} = w_{t-1} + y_{n(t)}x_{n(t)}
$$

对于$w_{t}$我们可以推导出：

$$
\begin{aligned}
   w_{f}^Tw_{t} &=  w_{f}^T(w_{t-1} + y_{n(t-1)}x_{n(t-1)}) \\
   &\geq w_{f}^Tw_{t-1} + \min\limits_{n}y_{n}w_{f}x_{n} \\
   &\geq w_{f}^Tw_{t-2} + 2 \cdot \min\limits_{n}y_{n}w_{f}x_{n} \\
   &... \\
   &\geq w_{f}w_{0} + T \cdot \min\limits_{n}y_{n}w_{f}x_{n} \\
   &\geq  T \cdot \min\limits_{n}y_{n}w_{f}x_{n}
\end{aligned}
$$

对于$||w_{t}||$我们可以推导出：

$$
\begin{aligned}
   ||w_{t}||^2 &= ||w_{t-1} + y_{n(t-1)}x_{n(t-1)} ||^2 \\
   &= ||w_{t-1}||^2 + 2\cdot y_{n(t-1)}w_{t}^Tx_{n(t-1)} + ||y_{n(t-1)}x_{n(t-1)}||^2 \\
   &\leq ||w_{t-1}||^2 + 0 + ||y_{n(t-1)}x_{n(t-1)}||^2 \\
   &\leq ||w_{t-1}||^2 + \max \limits_{n}||x_{n}||^2 \\
   &\leq ||w_{t-1}||^2 + 2 \cdot \max \limits_{n}||x_{n}||^2 \\
   &... \\
   &\leq ||w_{0}||^2 + T \cdot \max \limits_{n}||x_{n}||^2 \\
   &\leq T \cdot \max \limits_{n}||x_{n}||^2
\end{aligned}
$$

综合上述两个结论有：

$$
\begin{aligned}
  {w_{f}^T\over||w_{f}||}\cdot{w_{T}\over||w_{T}||} &= {T \cdot \min\limits_{n}y_{n}w_{f}^Tx_{n} \over ||w_{f}^T||\cdot||w_{t}||} \\
  &\geq {T \cdot \min\limits_{n}y_{n}w_{f}^Tx_{n} \over ||w_{f}^T|| \cdot \sqrt{T} \cdot \max\limits_{n}||x_{n}||} \\
  &\geq {\sqrt{T} \cdot \min\limits_{n}y_{n}w_{f}^Tx_{n} \over ||w_{f}^T|| \cdot \max\limits_{n}||x_{n}||} = \sqrt{T} \cdot constant
\end{aligned}
$$

又因为

$$
{w_{f}^T\over||w_{f}||}\cdot{w_{f}^T\over||w_{f}||}\leq1
$$

于是就有：

$$
\begin{aligned}
  {\sqrt{T} \cdot \min\limits_{n}y_{n}w_{f}^Tx_{n} \over ||w_{f}^T|| \cdot \max\limits_{n}||x_{n}||} \leq 1 \\
  \Leftrightarrow T \leq {\max\limits_{n}||x_{n}||^2\cdot||w_{f}^T||^2 \over (\min\limits_{n}y_{n}w_{f}^Tx_{n})^2} = {R^2 \over \rho^2}
\end{aligned}
$$

从上述推导中可知次数$T$是有上界的，经过有限次搜索可以找到将数据集$D$完全正确分开的线性超平面，所以PLA算法是会停止的。

## 四、Non-Separable Data

从第三部分可知，对于线性可分的数据，PLA算法是可以停下来，并且找到线性超平面的，但是对于非线性可分的数据，$w_{f}$并不存在，于是上述推导并不成立，PLA就不一定会停下来。事实上，现实中的大部分数据集$D$, 都会掺杂着噪声，这时机器学习的流程是：

![](/images/machine_learning_foundations/perceptron_03.png)

对于非线性的问题中，我们可以容忍一些点不能够被线性超平面正确分类，但这种情况越少发生约好，于是我们对PLA算法进行了适当了修改变成了Packet Algorithm, 下面是该算法的执行流程：

![](/images/machine_learning_foundations/perceptron_04.png)