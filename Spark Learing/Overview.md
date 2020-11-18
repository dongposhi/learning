
# 各种大数据工具的比较和分类


1. [批处理和刘处理](https://www.jianshu.com/p/5cc07eae1a0c)
    批处理模式中使用的数据集通常符合下列特征...
        有界：批处理数据集代表数据的有限集合
        持久：数据通常始终存储在某种类型的持久存储位置中
        大量：批处理操作通常是处理极为海量数据集的唯一方法
    流处理中的数据集是“无边界”的，这就产生了几个重要的影响：
        完整数据集只能代表截至目前已经进入到系统中的数据总量。
        工作数据集也许更相关，在特定时间只能代表某个单一数据项。
        处理工作是基于事件的，除非明确停止否则没有“尽头”。处理结果立刻可用，并会随着新数据的抵达继续更新。

2. [hadoop之Spark强有力竞争者Flink,Spark与Flink：对比与分析](https://zhuanlan.zhihu.com/p/45483483)

3. [FLINK vs SPARK](https://www.cnblogs.com/bonelee/p/6433485.html)

# 大数据处理工具的实例

1. 一个Spark 例子 [Spark SQL实现日志离线批处理](https://blog.csdn.net/shujuelin/article/details/80679760)