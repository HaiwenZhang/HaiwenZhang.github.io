---
title: 机器学习基石笔记：学习的类型
date: 2018-07-08 19:50:45
categories:
- Machine Learning
tags:
- 机器学习基石
comments: true
markup: mmark
---

### 一、 Learning with Different Output Space

二元分类问题：

$$
y=\{+1, -1\}
$$

输出只有两个，+1表示正类，-1表示负类。二元分类的问题很常见，例如信用卡发放、垃圾邮件判别等等。二元分类是机器学习最基本的问题。二元分类有线性模型和非线性模型，需要依据具体的问题选择对应的模型。下图展示了常见的二元分类：

<!--more-->

![](/images/machine_learning_foundations/types_of_learning_01.png)

除了二元分类，我们遇到的更多的是多元分类问题,也就是输出有多个。

$$
y=\{1, 2, 3, ..., k\}
$$

其中$k>2$，一般多元分类常应用在手写数字识别，图像识别，语音识别等等。

二元分类和多元分类都属于分类问题，它们的输出都是离散值。对于另外一种情况，比如预测房屋价格、股票收益多少等，这类问题的输出y=R，即范围在整个实数空间，是连续的。这类问题，我们把它叫做回归。

除了分类和回归问题，在自然语言处理等领域中，还会用到一种机器学习问题：结构化学习。结构化学习的输出空间包含了某种结构在里面，它的一些解法通常是从多分类问题延伸而来的，比较复杂。

![](/images/machine_learning_foundations/types_of_learning_02.png)


### 二、Learning with Different Data Label

监督学习：对于训练集$D$，任意的$x_n$都有对应的输出标签$y_n$。监督学习可以是二元分类，多元分类或者回归问题。

非监督学习：对于训练集$D$，任意的$x_n$是没有对应的输出标签$y_n$。非监督学习包括聚类、密度估计、异常值检测。

半监督学习：顾名思义它是介于监督学习和非监督学习之间。也就是对于训练集$D$，有的$x_n$有对应的输出标签$y_n$， 而有的$x_n$则没有对应的输出标签$y_n$。

增强学习：我们给模型或系统一些输入，但是给不了我们希望的真实的输出$y$，根据模型的输出反馈，如果反馈结果良好，更接近真实输出，就给其正向激励，如果反馈结果不好，偏离真实输出，就给其反向激励。

![](/images/machine_learning_foundations/types_of_learning_03.png)

### 三、Learning with Different Protocol

Batch learning: 获得的训练数据$D$是一批的，即一次性拿到整个$D$，对其进行学习建模，得到我们最终的机器学习模型。

Online learning: 是一种在线学习模型，数据是实时更新的，根据数据一个个进来，同步更新我们的算法。

Active learning: 让机器具备主动问问题的能力，例如手写数字识别，机器自己生成一个数字或者对它不确定的手写字主动提问。

![](/images/machine_learning_foundations/types_of_learning_04.png)

### 四、Learning with Different Input Space

Concrete features: 比如说硬币分类问题中硬币的尺寸、重量等；比如疾病诊断中的病人信息等具体特征。对机器学习来说concrete features 最容易理解和使用。

Raw features: 比如说手写数字识别中每个数字所在图片的mxn维像素值；比如语音信号的频谱等。raw features一般比较抽象，经常需要人或者机器来转换为其对应的concrete features，这个转换的过程就是Feature Transform。

Abstract features。比如某购物网站做购买预测时，提供给参赛者的是抽象加密过的资料编号或者ID，这些特征X完全是抽象的，没有实际的物理含义。所以对于机器学习来说是比较困难的，需要对特征进行更多的转换和提取。

![](/images/machine_learning_foundations/types_of_learning_05.png)