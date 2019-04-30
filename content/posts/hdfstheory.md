---
title: HDFS读写浅析
date: 2018-04-27 14:29:09
categories:
- Hadoop
tags:
- Hadoop
comments: true
---
## 一、HDFS 架构

<!--more-->

![](https://hadoop.apache.org/docs/r1.2.1/images/hdfsarchitecture.gif)


由上图可知HDFS主要有三个部分构成，分别为：Client、NameNode、DataNode.这三个部分的主要作用如下:

#### 1) Client: 也就是客户端
1. 文件切分，文件上传HDFS的时候，Client将文件切分成一个一个的block，然后进行存储。
2. 与NameNode交互，获取文件的位置信息。
3. 与DataNode交互，读取或者写入数据。
4. 提供一些命令来管理和访问HDFS。

#### 2) NameNode: 也就是Master
1. 管理HDFS的命名空间
2. 管理数据块(Block)的映射信息
3. 配置副本策略
4. 处理客户端读写请求

#### 3) DataNode: 也就是Slave.
1. 存储实际的数据块(Block)
2. 执行数据块的读/写操作

## 二、HDFS文件写入流程

![](https://wx2.sinaimg.cn/mw1024/b6d9ac4egy1fqrbgsja6tj21kw0t0wlj.jpg)

1. 客户端通过Distributed FileSystem模块向NameNode请求上传文件，NameNode检查目标文件是否已存在，父目录是否存在；
2. Namenode返回是否可以上传；
3. 客户端向NameNode请求第一个Block上传到哪几个DataNode服务器上；
4. NameNode返回3个DataNode节点，分别为dn1、dn2、dn3；
5. 客户端通过FSDataOutputStream模块请求dn1上传数据，dn1收到请求会继续调用dn2，然后dn2调用dn3，将这个通信管道建立完成；
6. dn1、dn2、dn3逐级应答客户端；
7. 客户端开始往dn1上传第一个block（先从磁盘读取数据放到一个本地内存缓存），以packet为单位，dn1收到一个packet就会传给dn2，dn2传给dn3，dn1每传一个packet会放入一个应答队列等待应答；
8. 当一个Block传输完成之后，客户端再次请求NameNode上传第二个Block的服务器（重复执行3-7步直至所有的切分出来的Block上传完毕）。

## 三、HDFS文件读取流程

![](https://wx1.sinaimg.cn/mw1024/b6d9ac4egy1fqrc1nrcw0j21kw0r6tek.jpg)

1. 客户端通过Distributed FileSystem向NameNode请求下载文件，NameNode通过查询元数据，找到文件块所在的DataNode地址；
2. 挑选一台DataNode（就近原则，然后随机）服务器，请求读取数据；
3. DataNode开始传输数据给客户端（从磁盘里面读取数据输入流，以packet为单位来做校验）；
4. 客户端以packet为单位接收，先在本地缓存，然后写入目标文件。