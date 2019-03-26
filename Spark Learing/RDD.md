[深入理解Spark RDD抽象模型](https://github.com/linbojin/spark-notes/blob/master/rdd-abstraction.md)


**RDD** 以**Partition**的形式分部在各个Node 上，每个Node 上可以有 多个**Partition**（**Partition**不可以跨Node）。每个Task 对应一个**Partition**。**Task**相互独立，可以**并行**计算


**Lineage** includes the data's origins, what happens to it and where it moves over time.[1] Data lineage gives visibility while greatly simplifying the ability to trace errors back to the root cause in a data analytics process [wiki](https://en.wikipedia.org/wiki/Data_lineage)

**Lineage Graph**也可以说成**Dependency Graph**

窄依赖里面的运算符可以被pipeline在一起（不需要shuffle），用**一个Task**直接计算

- 一个典型的Spark 基于RDD 的数据处理过程

![RDD 的一个典型使用方式](https://pic4.zhimg.com/v2-ef213d25d637aa989b1e0ffea319d903_r.jpg)




