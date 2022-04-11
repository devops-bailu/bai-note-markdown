# Apache Flink

## Flink 是什么

Flink 官方网站`https://flink.apache.org/`

**Apache Flink** is a framework and distributed processing engine for stateful computations over *unbounded* and *bounded* data streams.

Apache Flink 是一个分布式处理引擎和框架，用于对 `无界和有界数据流`进行状态计算

## 为什么要用 Flink

具备批处理和流处理能力

流处理更加符合生产实际

Flink 的优点：

- 低延迟
- 高吞吐
- 准确的结果和好的容错性

## Flink 的主要特点

#### 1. Flink 具备良好的流处理能力

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//flink-base202204111404787.png)

#### 2. Flink 的分层API（4层API）

Flink具备良好的分层API，可以按需选择，用于不同业务需求

- 越靠近顶层越抽象，表达含义越简单明了，使用起来越方便，但是能做的事情就会越有限
- 越靠近底层越具体，表达含义越丰富，使用起来越灵活，可做的事情就会越多

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//flink-base202204111415471.png)

## Flink 和 Spark Streaming

#### 1. 数据模型

- Spark 采用 RDD 模型，spark streaming 的 DStream 实际上就是一组一组的小批量数据 RDD 的集合
- Flink 基本数据类型是数据流，以及事件（Event）序列

**备注：**

RDD（Resilient Distributed Datasets）：分布式弹性数据集

#### 2. 运行时架构

- Spark 是批计算，将 DAG 划分为不同的 stage，一个完成以后才可以计算下一个
- Flink 是标准的流处理模式，一个事件在一个节点处理完成以后，可以直接发送到下一个节点

**备注：**

DAG（Direct Acyclic Graph）：有向无环图