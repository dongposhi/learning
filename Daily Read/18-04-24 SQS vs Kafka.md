


https://www.opsclarity.com/evaluating-message-brokers-kafka-vs-kinesis-vs-sqs/

这篇文章对比了SQS 和Kafka ，他的应用最终迁移到了Kafka

**应用背景：** 
OpsClarity 提供运维数据metric 收集处理（stream metrics data）。类似于Amazon 的click stream。对queue 的需求在于能够提供
	1. 实时的数据处理
	2. queue 提供对数据进行平滑化的中间层。避免spike对数据处理的影响。
功能需求：
	1. 不同客户间的数据隔离
	2. 1.  Guarantee availability of our monitoring solution all the time by guarding our data pipeline resources against a big surge of data from “misbehaving” hosts from one customer.

一开始的选择是SQS：快捷，能够满足功能需求

遇到的问题：
	1. 业务要求对raw data 进行多重处理。而SQS 只能消费一次，为满足该需求西药duplicate SQS


重新选择solution
1. Kinesis
2. Kafka

结论：
	1. Kinesis 贵，
	2. Kinesis 在sharding 方案上表现不好。
	3. Kafka 提供的topic 订阅解决了业务需求问题。
	4. Kafka 在吞吐量方面性能优异。
	5. Kafka 支持 replay message
	6. 安装运维简单。


此文的比较偏简单。没有太深入。有几点思考
	1. SNS +SQS 能够解决OpsClarity 的新需求，不知为什么没有提。
	2. AWS 的方案在长期运维中的优势没有被提及。Kafka 是需要自己运维的。
	


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwODA4NjAyNCwxNDQ0MDM5MDA3LDE3OD
I3MDEzNjUsLTExNDgyOTIxMzBdfQ==
-->