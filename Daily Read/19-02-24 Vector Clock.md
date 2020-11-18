# Vector Clock


Vector Clock 是分布式系统的以及基础算法。他解决的分布式系统内的event 顺序，也就是 版本问题。通常分布式数据库系统可以基于这个算法来完成transaction 的实现。

[Logic clock](https://en.wikipedia.org/wiki/Logical_clock) 是 Vector Clock 的思想源泉。[Ref2] 指出了 Logic clock 的局限（没看懂）。**逻辑时钟就是timestamp或者单调递增的逻辑时钟，只能保证事件在分布式系统中的半序，它能区分有因果关联的事件的序，但是不能确定没有因果关系的事件间的序。而向量逻辑时钟是全序的。**

Vector clock 是Dynamo（不是DynamoDB） 中提出的分布式数据库Transaction 结局方案。但是Amazon 已经放弃了这个方案。一个明显的问题是 Vector Clock 不能满足Scale 的要求。代之以synchronous replication 方案。 **偏重于理论，不适合工程化**

Cassandra 也曾经采用过Vector Clock。后放弃之。（曾经，不确定现在）退回去采用了NTP 时间的方案。

本质上解决的也是一致性问题。

## 几种其他解决问题的思路：
1. 基于NTP时钟 （有几十ms 的误差）
2. synchronous replication （one leader writter）


## Ref
1. https://en.wikipedia.org/wiki/Vector_clock
2. https://lrita.github.io/2018/10/24/lamport-logical-clocks-vector-lock/
3. http://www.cnblogs.com/foxmailed/p/4985848.html
4. [为什么分布式数据库需要使用Vector Clock？](https://www.zhihu.com/question/19994133)