---
title: Spark入门：核心概念RDD
date: 2018-05-01 12:13:53
categories:
- Spark
tags:
- Spark
- RDD
comments: true
---
## 1.1 什么是Spark？

Spark最初由美国加州伯克利大学（UCBerkeley）的AMP（Algorithms, Machines and People）实验室于2009年开发，是基于内存计算的大数据并行计算框架，可用于构建大型的、低延迟的数据分析应用程序。Spark在诞生之初属于研究性项目，其诸多核心理念均源自学术研究论文。2013年，Spark加入Apache孵化器项目后，开始获得迅猛的发展，如今已成为Apache软件基金会最重要的三大分布式计算系统开源项目之一（即Hadoop、Spark、Storm）。

<!--more-->

Spark具有以下几个特性：
1. 运行速度快：Spark使用先进的DAG（Directed Acyclic Graph，有向无环图）执行引擎，以支持循环数据流与内存计算，基于内存的执行速度可比Hadoop MapReduce快上百倍，基于磁盘的执行速度也能快十倍；
2. 应用性好：Spark支持使用Scala、Java、Python和R语言进行编程，简洁的API设计有助于用户轻松构建并行程序，并且可以通过Spark Shell进行交互式编程；
3. 通用性强：Spark提供了完整而强大的技术栈，包括SQL查询、流式计算、机器学习和图算法组件，这些组件可以无缝整合在同一个应用中，足以应对复杂的计算；
4. 随处运行：Spark可运行于独立的集群模式中，或者运行于Hadoop中，也可运行于Amazon EC2等云环境中，并且可以访问HDFS、Cassandra、HBase、Hive等多种数据源。

## 1.2 Spark与MapReduce比较

Spark借鉴Hadoop MapReduce发展而来的，继承了其分布式并行计算的优点以及改进了MapReduce的缺陷,具体如下：
1. Spark把中间数据放在了内存中，迭代的效率高，而MapReduce将计算结果放在磁盘中，这样会影响整体的运行速度。Spark使用的是DAG分布式并行计算的编程模型，减少了迭代过程中数据的落地提高了计算效率。
2. Spark容错性高。Spark使用了弹性分布式数据集(RDD)，他是分布在一组节点中的只读对象，具有很高的容错性
3. Spark更加通用。Hadoop只提供了Map和Reduce两种操作，Spark则提供了Transformations和Actions两大类操作。

## 2.1 RDD概念

一个RDD就是一个分布式对象集合，本质上是一个只读的分区记录集合，每个RDD可以分成多个分区，每个分区就是一个数据集片段，并且一个RDD的不同分区可以被保存到集群中不同的节点上，从而可以在集群中的不同节点上进行并行计算。RDD有五大特点:
1. 有一个分片列表；
2. 一个函数计算每一个分片；
3. 对其他RDD的依赖列表；
4. 可选：key-value型的RDD是根据哈希来分区的；
5. 可选：每一分片的优先计算位置。

## 2.2 RDD的简单执行过程

1. RDD读入外部数据源（或者内存中的集合）进行创建；
2. RDD经过一系列的“转换”操作，每一次都会产生不同的RDD，供给下一个“转换”使用；
3. 最后一个RDD经“行动”操作进行处理，并输出到外部数据源（或者变成Scala集合或标量）。

## 2.3 RDD的分类

RDD的操作算子包括两类，一类叫做transformations，它是用来将RDD进行转化，构建RDD的血缘关系；另一类叫做actions，它是用来触发RDD的计算，得到RDD的相关计算结果或者将RDD保存的文件系统中。

## 2.4 RDD的依赖

RDDs通过操作算子进行转换，转换得到的新RDD包含了从其他RDDs衍生所必需的信息，RDDs之间维护着这种血缘关系，也称之为依赖。如下图所示，依赖包括两种，一种是窄依赖，RDDs之间分区是一一对应的，另一种是宽依赖，下游RDD的每个分区与上游RDD(也称之为父RDD)的每个分区都有关，是多对多的关系。
![](https://wx4.sinaimg.cn/mw1024/b6d9ac4egy1fr0dhvkahsj21720oytcq.jpg)

## 3 RDD是如何容错的？
1. RDD是不可变的，对RDD做操作时只会生成一个新的RDD而不是更改内部的数据；
2. RDD实现了基于Lineage的容错机制。RDD之间的转换关系，构成Lineage。在部分计算结果丢失时，只需要根据这个Lineage重算即可；
3. 使用Checkpoint机制，当Lineage过长时，Spark集群需要花费很多时间将DAG的结果计算出来，如果中间有计算出现错误或者数据丢失，那么Spark根据RDD的依赖关系重新计算，这样会耗费很长时间。于是我们就可以用checkpoint机制将中间重要的结果持久化到高可用的存储系统(一般为HDFS)。