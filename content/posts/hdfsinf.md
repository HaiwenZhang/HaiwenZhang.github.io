---
title: HDFS 简介
date: 2018-04-23 15:56:22
categories:
- Hadoop
tags:
- Hadoop
comments: true
---
## 1 什么是HDFS?

HDFS是一个分布式文件系统，适合部署在廉价的机器上。HDFS适合一次写入，多次读取的场景，且不支持文件的修改。

<!--more-->

## 2 HDFS的优缺点
优点：

1. 高容错性：数据自动保存多个副本，副本丢失后，自动恢复
2. 适合大数据处理：能够处理数据规模达到GB、TB、甚至PB级别的数据以及处理百万规模以上的文件数量
3. 流式数据访问：一次性写入，多次读取，保证数据一致性。

缺点：

1. 不适合低延迟数据访问场景：比如毫秒级，低延迟与高吞吐率
2. 不适合小文件存取场景:占用NameNode大量内存。寻址时间超过读取时间。
3. 不适合并发写入，文件随机修改场景：一个文件只能有一个写者


## 3 HDFS命令行操作
#### 1) 基本语法
```bash
hadoop fs <cmd>
```

#### 2) 常用命令实操
(1) -ls: 显示目录信息
```bash
hadoop fs -ls /
```

(2) -mkdir: 在hdfs上创建目录
```bash
$ hadoop fs -mkdir -p /user/haiwen/test
```

(3) -copyFromLocal: 从本地文件系统中拷贝文件到hdfs
```bash
$ hadoop fs -copyFromLocal README.txt /user/haiwen/test/README.txt
```

(4) -copyToLocal: 从hdfs拷贝到本地
```bash
$ hadoop fs -copyToLocal /user/haiwen/test/README.txt README.txt
```

(5) -cp: 从hdfs的一个路径拷贝到hdfs的另一个路径
```bash
$ hadoop fs -cp /user/haiwen/test/README.txt /README.txt
```

(6) -mv: 在hdfs目录中移动文件
```bash
$ hadoop fs -mv /user/haiwen/test/README.txt /README.txt
```

(7) -appendToFile: 追加一个文件到已经存在的文件末尾
```bash
$ hadoop fs -appendToFile hello.txt /user/haiwen/test/README.txt
```

(8) -rm: 删除文件或文件夹
```bash
$ hadoop fs -rm -R /user/haiwen/test
```

