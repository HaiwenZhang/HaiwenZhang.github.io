---
title: Spark入门：可视化RDD的算子(1)
date: 2018-05-02 13:24:51
categories:
- Spark
tags:
- Spark
- RDD
comments: true
---

## 1. Spark算子：map

map是对RDD中的每个元素都执行一个指定的函数来产生一个新的RDD，新RDD叫作MappedRDD(this, sc.clean(f))。任何原RDD中的元素在新RDD中都有且只有一个元素与之对应。

```scala
val x = sc.parallelize(Array("b", "a", "c"))
val y = x.map(z => (z, 1))
println(x.collect().mkString(","))
println(y.collect().mkString(","))

// x: ["b", "a", "c"]
// y: [("b", 1), ("a", 1), ("c", 1)]
```

<!--more-->

![](/images/spark_map_visual.png)

## 2. Spark算子：flatMap

类似于map，但是每一个输入元素，会被映射为0到多个输出元素（因此，func函数的返回值是一个Seq，而不是单一元素）。

```scala
val x = sc.parallelize(Array(1, 2, 3))
val y = x.flatMap(n => Array(n, n*100, 42))
println(x.collect().mkString(","))
println(y.collect().mkString(","))

// x: [1, 2, 3]
// y: [1, 100, 42, 2, 200, 42, 3, 300, 42]
```

![](/images/spark_flatmap_visual.png)

## 3. Spark算子：filter

filter的功能是对元素进行过滤，对每个元素应用f函数，返回值为true的元素在RDD中保留，返回为false的将过滤掉。

```scala
val x = sc.parallelize(Array(1, 2, 3))
val y = x.flatMap(n => n%2 == 1))
println(x.collect().mkString(","))
println(y.collect().mkString(","))

// x: [1, 2, 3]
// y: [1, 3]
```

![](/images/spark_filter_visual.png)

##### 参考
[Visual API](http://training.databricks.com/visualapi.pdf)