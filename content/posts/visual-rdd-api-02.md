---
title: Spark入门：可视化RDD的算子(2)
date: 2018-05-03 19:37:39
categories:
- Spark
tags:
- Spark
- RDD
comments: true
---

## 1. Spark算子：groupBy

groupBy(f) f返回key，传入的RDD的各个元素根据这个key进行分组。

```scala
val x = sc.parallelize(Array("John", "Fred", "Anna", "James"))
val y = x.groupBy(w => w.charAt(0))
println(x.collect().mkString(","))
println(y.collect().mkString(","))

// x: ["John", "Fred", "Anna", "James"]
// y: [('A',['Anna']),('J',['John', 'James']),('F',['Fred'])]
```

<!--more-->

![](/images/spark-groupby-01.png)
![](/images/spark-groupby-02.png)


## 2. Spark算子：reduceByKey

在一个(K,V)的RDD上调用，返回一个(K,V)的RDD，使用指定的reduce函数，将相同key的值聚合到一起，reduce任务的个数可以通过第二个可选的参数来设置。

```scala
val words = Array("one", "two", "two", "three", "three", "three")
val wordPairsRDD = sc.parallelize(words).map(word => (word, 1))
val wordCountsWithReduce = wordPairsRDD.reduceByKey(_ + _).collect()

// Array((two,2), (one,1), (three,3))
```
![](/images/spark-reducebykey.png)


## 3. Spark算子：groupByKey

在一个(K,V)的RDD上调用，返回一个(K,V)的RDD，使用指定的函数，将相同key的值聚合到一起，与reduceByKey的区别是只生成一个sequence。

```scala
val words = Array("one", "two", "two", "three", "three", "three")
val wordPairsRDD = sc.parallelize(words).map(word => (word, 1))
val wordCountsWithGroup = wordPairsRDD.groupByKey().map(t => (t._1, t._2.sum)).collect()

// Array((two,2), (one,1), (three,3))
```
![](/images/spark-groupbykey.png)
