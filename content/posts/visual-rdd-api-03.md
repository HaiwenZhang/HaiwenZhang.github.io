---
title: Spark入门：可视化RDD的算子(3)
date: 2018-05-04 19:37:39
categories:
- Spark
tags:
- Spark
- RDD
comments: true
---

## 1. Spark算子：mapPartitions

类似于map，但独立地在RDD的每一个分片上运行，因此在类型为T的RDD上运行时，func的函数类型必须是Iterator[T] => Iterator[U]。假设有N个元素，有M个分区，那么map的函数的将被调用N次,而mapPartitions被调用M次,一个函数一次处理所有分区。

```scala
val x = sc.parallelize(Array(1,2,3), 2)
def f(i:Iterator[Int])={ (i.sum,42).productIterator }
val y = x.mapPartitions(f)

// glom() flattens elements on the same partition
val xOut = x.glom().collect()
val yOut = y.glom().collect()

// x: Array(Array(1), Array(2, 3))
// y: Array(Array(1, 42), Array(5, 42))
```

<!--more-->

![](/images/spark-mapPartitions.png)


## 2. Spark算子：mapPartitionsWithIndex

类似于mapPartitions，但func带有一个整数参数表示分片的索引值，因此在类型为T的RDD上运行时，func的函数类型必须是(Int, Interator[T]) => Iterator[U]。

```
val x = sc.parallelize(Array(1,2,3), 2)

def f(partitionIndex:Int, i:Iterator[Int]) = {
  (partitionIndex, i.sum).productIterator
}
val y = x.mapPartitionsWithIndex(f)

// glom() flattens elements on the same partition
val xOut = x.glom().collect()
val yOut = y.glom().collect()

// x: Array(Array(1), Array(2, 3))
// y: Array(Array(0, 1), Array(1, 5))
```
![](/images/spark-mapPartitionsWithIndex.png)


## 3. Spark算子：sample

sample(withReplacement: Boolean, fraction: Double, seed: Long = Utils.random.nextLong)
以指定的随机种子随机抽样出数量为fraction的数据，withReplacement表示是抽出的数据是否放回，true为有放回的抽样，false为无放回的抽样，seed用于指定随机数生成器种子。例子从RDD中随机且有放回的抽出50%的数据，随机种子值为3（即可能以1 2 3的其中一个起始值）。

```scala
val x = sc.parallelize(Array(1, 2, 3, 4, 5))
val y = x.sample(false, 0.4)

println(y.collect().mkString(", "))

// x: 1, 2, 3, 4, 5
// y: 3, 4
```
![](/images/spark-sample.png)


## 4. Spark算子：takeSample

takeSample(withReplacement: Boolean, num: Int, seed: Long = Utils.random.nextLong))
和Sample的区别是：takeSample返回的是最终的结果集合

```scala
val x = sc.parallelize(Array(1, 2, 3, 4, 5))
val y = x.takeSample(false, 2)

println(y.collect().mkString(", "))

// x: 1, 2, 3, 4, 5
// y: 1, 4
```
![](/images/spark-takeSample.png)

