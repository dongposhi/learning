

- https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html
- https://w.amazon.com/bin/view/DynamoDB_Operational_Best_Practices


### 重要的TIPS
1. 在开始设计之前，一定要**充分理解业务（了解所有的访问方式）**，理解数据的访问方式。
	1. 一旦表被创建以后，在想修改的难度极大。 
		a. 不知道是否能够创建LSI
		b. 如果数据量很大的话，创建GSI 会需要很长时间（以天为单位）
   2. 对于应用而言，尽可能的少创建表。一般一个应用只需要一个表。
2. 一个table 只能有5个GSI和5个LSI，多用GSI
3. 对于NOSQL 而言， data shape 实在设计的时候就定论，而不是想RDBM一样可以在查询阶段reshape。
4. GSI 只提供**最终一致性**。
5. LSI **只能**在创建表的时候定义。
6. LSI 推荐使用组合键。
7. dynamodb calculators:  https://s3.us-east-2.amazonaws.com/dynamodb-calculators-public/index.html
8. sparse index （稀疏索引）就是意味着大量的插入记录不包含索引的sort key
	1. 文档举得例子是isOpen flag 在closed 的时候删掉该属性。特别适用于 等待 EVENT_CLEARANCE_NOTIFICATION 的例子。
9. 对于时间为主键的应用，而且写入是随时间发生的。建议根据时间划分多表，避免partition 分配WCU。数据类似于倾斜的树 ，而WCU 是平衡分配的，所以采用这种方案。
10.物理partition的写入最大是1000WCU，每个partition 是10G，超过了就会split 
### 自身感悟
1. dynamoDB 就是一个在线交易系统，擅长的是快速记录，不怕记多和啰嗦。
2. 分partition 是为了提供并行查询的能力。
3. 10G /partition
4. DynamoDB 是鼓励在一个table 中存各种类型的数据等，所以在设计数据表的时候，每个attribute 的name 设计可以含糊。‘’，当然不含糊也行，反正不要求对
	我觉得是不是可以推断出 pk，sk 都最好是string 才能保证最大的通用性。 
5. 

### FAQ
**Q：** 如果一个table 的 provisioned RCU，WCU 很小，那么开始的时候partition 按公式计算只有一个。随着数据的累计，该partition 的数据很快超过了10G。DynamoDB 这时候会自动进行partition split。我的问题在于，是否意味这partition key 的寻址算法随之调整？数据是否会平均分配到各个partition？如果一开始在设计表的时候给一个很大的WCU，RCU，此时计算的partition 会较大。随后将WCU，RCU 降到一个很低的值（此时计算只是有一个partition），此时partitionId 到物理partition 的映射关系还是按照开始时设计的进行吗？还是随之调整？
**A:** 简而言之，partition 只会增加，不会减少。
 https://w.amazon.com/bin/view/DynamoDB_Operational_Best_Practices/#_Toc463916832


### Open Qustions
1. 我有一个很大的表，已经存了很多的数据。这是后想创建一个sparse 的GSI， 是否也会很废时间？根据提示，创建一个新的GSI 需要 10+天。
4. 在DynamoDB的best practice 中建议保证partition key 的均匀分布，在不是到 partition 寻址算法的情况下，怎么做到partition key 的选择呢？
5. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDcwMDEwOTY2LC0xMzgwOTQ3OTYyLC0xND
MyMjk4NzAxLDE1NTA1MTY3NjIsMTMxMjg2MTcxLDIwMjIxNTQx
MDQsNDUxMDMyMjE0XX0=
-->