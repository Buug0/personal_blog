---
title : 'Spark基本架构'
date : 2026-05-12T10:56:01+08:00
draft: true
cover: "images/kita.png"
banner: "images/8.png"
---
spark集群架构和mapreducex集群架构相似，都是主从结构。

## RDD
MapReduce 框架采用非循环式的数据流模型，把中间结果写入到 HDFS 中，带来了大量的数据复制、磁盘 IO 和序列化开销。且这些框架只能支持一些特定的计算模式(map/reduce)，并没有提供一种通用的数据抽象。因此出现了RDD这个概念。

RDD(Resilient Distributed Dataset)叫做弹性分布式数据集，是 Spark 中最基本的数据抽象，代表一个不可变、可分区、里面的元素可并行计算的集合。
   - Resilient ：它是弹性的，RDD 里面的中的数据可以保存在内存中或者磁盘里面；
   - Distributed ： 它里面的元素是分布式存储的，可以用于分布式计算；
   - Dataset: 它是一个集合，可以存放很多元素。
 
RDD 的算子分为两类:
   - Transformation转换操作:返回一个新的 RDD
   - Action动作操作:返回值不是 RDD(无返回值或返回其他的)
   - RDD 不实际存储真正要计算的数据，而是记录了数据的位置在哪里，数据的转换关系(调用了什么方法，传入什么函数)。
   - RDD 中的所有转换都是惰性求值/延迟执行的，也就是说并不会直接计算。只有当发生一个要求返回结果给 Driver 的 Action动作时，这些转换才会真正运行。之所以使用惰性求值/延迟执行，是因为这样可以在 Action 时对 RDD 操作形成 DAG有向无环图进行 Stage 的划分和并行优化，这种设计让 Spark 更加有效率地运行。
