# Overview
Spark 应用的主程序只有一个，在**Driver** 上执行。而数据处理又是在cluster 的**Executors**中分布式地被处理的。处理逻辑是如何被分布到各个系统计算资源的呢？

Spark 的处理是以数据集为中心的（i.e. RDD）

Spark 应用的主程序执行过程可以看做是两个阶段：编译过程和执行过程

- **编译过程（transform)** 就是把处理流程编译为Task，这个过程不处理数据，只是把处理方法封装成Task。这也就是所谓的 **Lazy** 执行

- **执行过程（action)** 这个算子的执行会触发对已经编译过的任务进行分发，reduce得到结果。


**Executor** 在收到Task 数据后，将Task 反序列化就得到了需要执行的闭包（包括必要的数据和处理逻辑（i.e. 函数）））。

# 序列化
## ClosureCleaner

在对闭包进行序列化的时候，Spark 实现了一个类 **ClosureCleaner** 完成了对闭包的必要清理，使其序列化体积更小，更不容易出错。

https://blog.csdn.net/weixin_33858485/article/details/87147909

这篇文章准确的分析了ClosureCleaner 的作用

###Summary
spark里面，大量使用了一个方法, ClosureCleaner.clean()

quora上有一篇参考文章 https://www.quora.com/What-does-Closure-cleaner-func-mean-in-Spark

大概意思，scala支持闭包(jvm上的闭包当然也是一个对像)，闭包会把它对外的引用(闭包里面引用了闭包外面的对像)保存到自己内部，这个闭包就可以被单独使用了，而不用担心它脱离了当前的作用域；

但是在spark这种分布式环境里，这种作法会带来问题，如果对外部的引用是不可serializable的，它就不能正确被发送到worker节点上去了；还有一些引用，可能根本没有用到，这些没有使用到的引用是不需要被发到worker上的； ClosureCleaner.clean()就是用来完成这个事的；ClosureCleaner.clean()通过递归遍历闭包里面的引用，检查不能serializable的, 去除unused的引用；

这个方法在SparkContext中用得很多，对rpc方法，只要传入的是闭包，基本都会使用这个方法，它可以降低网络io,提高executor的内存效率

# 反序列化 
\<TBD>