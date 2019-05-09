


AWS Kinesis  理解

https://www.sumologic.com/blog/devops/kinesis-streams-vs-firehose/

# What's Kinesis

- Kinesis 是一个分布式（支持Sharding）的 Data Stream
	- 功能上是data stream
	- 特色是分布式支持，可以线性scale out
- Kinesis 功能上可以类比于 Kafka。于 Kafka不同的是 Kinesis 是一项服务，而Kafka 是一个软件（需要自己搭建，运维）
	- https://www.quora.com/Which-of-Amazon-Kinesis-and-Apache-Kafka-is-the-more-proven-and-high-performance-oriented  
# What's difference between Kinesis and SQS
- 我在一个内部分享中看到了一个最简洁的回答：
  - kinesis 是一个buffer
    - 我的认知就是data stream 是一个有时限的临时缓存，缓存是为了提供一个即时的处理窗口，与SQS 完全不是一个目的。
- https://stackoverflow.com/questions/26623673/why-should-i-use-amazon-kinesis-and-not-sns-sqs/33797510#33797510  有两个评分不高的回答更得我心。
	- Kinesis solves the problem of map part in a typical map-reduce scenario for streaming data. While SQS doesn't make sure of that. If you have streaming data that needs to be aggregated on a key, kinesis makes sure that all the data for that key goes to a specific shard and the shard can be consumed on a single host making the aggregation on key easier compared to SQS
	- The pricing models are different, so depending on your use case one or the other may be cheaper. Using the simplest case (not including SNS). 简单说数据量小 （G level/day） SQS 更便宜，数据量大（T level / day） Kinesis 更划算。
	- 其他方面：
		- Kinesis 支持routing (shard based)
		-  Kinesis 支持order
		- Kinesis 支持multiConsumer
# What's difference of “Amazon Kinesis Data Streams” and “Amazon Kinesis Data Firehose”
- 通俗的说，DataStream 只是一个data stream，他的两端（producer，和 consumer）都是使用者自己去构建的。DataStream 很纯粹的是个pipeline。 而Firehose 是提供了终结点(consumer) 的DataStream。这个终结点指定了是S3 或这Redshift （不可以是其他的东西）
- [Kinesis Streams vs Firehose](https://www.sumologic.com/blog/devops/kinesis-streams-vs-firehose/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MTkxNTA5NzMsMTUxNTY0NzY2Nl19
-->