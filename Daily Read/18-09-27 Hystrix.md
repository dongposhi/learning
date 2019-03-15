


https://github.com/Netflix/Hystrix/wiki/How-it-Works

Hystrix 实际上是一个client 侧的 library。这在Amazon 是通过JavaClient library 来完成的。在使用的时候通过配置各种参数来达成目的。

## components

- **ExecutionResult**  这个类的设计是保存执行结果。
	- 执行结果是标识成功或者异常，如果异常还有保存异常的Exception
	- 保存开始时间和执行时间
	- 不可变，每个set 函数都返回一个新的对象。
		- for cache 特性
	- EventCounts 设计为BitSet 为核心的数据结构

## Request Collapse and Request Cache

Request Collapse 是在客户端以一个时间窗口内累计多个request 然后batch 在一个进程内发送请求。这样可以减少客户端的线程和连接的使用。

不好的地方是，这样会加剧服务端的瞬时处理压力。

只有在特殊的场景下才适用，意思不大。

## 文档本身
workflow，sequence 图都可圈可点。很好的解释了high level design


## 延展阅读
* **bulkhead patter**    隔水舱模式   
	* https://docs.microsoft.com/en-us/azure/architecture/patterns/bulkhead
	* https://stackoverflow.com/questions/30391809/what-is-bulkhead-pattern-used-by-hystrix
	* 这个模式的选择实际上一个隔离和效率的平衡问题。
	* 这个模式类似于多租户对资源的隔离
	* thread isolation and semaphore isolation 两种解决方案
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMTIzNzY5NzUsLTIwOTU2MDA2OTUsLT
EzOTc3NzYwMzgsLTI5MzgzMDddfQ==
-->