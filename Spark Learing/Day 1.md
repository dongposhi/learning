

DataBrick provide community cluster for trying Spark
My acccount: shidp@amazon.com/Shi_xxx1x
https://community.cloud.databricks.com/?o=3996460956714541#

## Terms
### DataFrame， DataSet

https://databricks.com/blog/2016/07/14/a-tale-of-three-apache-spark-apis-rdds-dataframes-and-datasets.html

https://www.jianshu.com/p/a19486f5a0ea 

https://zhuanlan.zhihu.com/p/29830732

From 2.0, DataFrame 被合并进DateSet（For scala and Java）。DataFrame表示为DataSet[Row]，即DataSet的子集。

**A DataFrame is a  _Dataset_  organized into named columns.**

DataFrame可以看做分布式Row对象的集合，其提供了由列组成的详细模式信息，  
使其可以得到优化。  其中数据被组织成命名的列。

DataFrame只是知道字段，但是不知道字段的类型，所以在执行这些操作的时候是没办法在编译的时候检查是否类型失败的，比如你可以对一个String进行减法操作，在执行的时候才报错，而DataSet不仅仅知道字段，而且知道字段类型，所以有更严格的错误检查。

DataFrame 不仅有比RDD更多的算子，还可以进行执行计划的优化。

**使用API尽量使用DataSet ,不行再选用DataFrame，其次选择RDD。**

### Shuffle
exchange the partition across the cluster. It's action of wide dependency.
Shuffle描述着数据从map task输出到reduce task输入的这段过程。在分布式情况下，reduce task需要跨节点去拉取其它节点上的map task结果。这一过程将会产生网络资源消耗和内存，磁盘IO的消耗。

### Stage & Task
发生箭头交叉就形成一个stage，其中与伴随这shuffle操作， stage 是 taskset
### 算子
https://blog.csdn.net/csdnliuxin123524/article/details/81875800
算子的含义：

1.  hadoop只有map、Reduce这两个算子
2.  Spark提供了很多算子：

### 窄依赖，宽依赖
- 只是针对transformation 而言

## FAQ
### 1. Does the spark has daemon process on the cluster? both for driver and executor?
From my understanding, there's no such daemon process. Each Spark application has its own expectation for resources. It's defined in configuration for each application. When the application is submitted to the cluster manager(local, yarn, mesos). The requirement for resources is submitted to yarn, mesos. They (yarn, mesos) will assign resource to support the application.

Above understanding may not be true. In the test, when you create a cluster, the cluster has a SparkUI. A cluster should be connected with many spark sessions.

When I test spark on local manager. After I closed the sparksession by stopping pyspark. SparkUI's url is not accessable any more. If I started 2 spark sessions. only when both of them are stopped, the SparkUI's url became inaccessible.

### 什么是spark 中的资源队列？怎么共享？在什么范围内共享？

### Spark drive 在向资源管理器申请资源执行Executor 的时候，executor 所需要的执行程序文件是如何、何时部署到executor 上的？如果executor 是运行在JVM 上的，如何保证底层资源安装了相关的软件？

提交 spark 应用时，spark 会把应用代码分发到所有的 worker 上面，应用依赖的包需要在所有的worker上都存在，有两种解决 worker 上相关包依赖的问题：

-   选用一些工具统一部署 spark cluster；
-   在提交 spark 应用的时候，指定应用依赖的相关包，把 应用代码，应用依赖包 一起分发到 worker；
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA2OTczNzYwLC0xMTYxMDIzMjA0LC0xMj
M4OTk3NjE1LDY2NjQ3OTE2Niw2ODEyMTA4NzgsLTE0MjUwMTM3
NDIsNDkzODc3OTQyLC04NDM4NTk0MjIsLTIwMDI4ODgzMzUsMT
AyMTkzNjg3NCwxNDY5NzA1NjI4LC0zMDAzOTI4NjQsLTI4MjIz
NjM0LDExNTc4MzEzNzEsLTEzNzQ4Nzc5MjgsNzMwOTk4MTE2XX
0=
-->