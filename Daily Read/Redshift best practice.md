
# Overview
- Redshift 不能简单理解为 SQL database
- Redshift 在大规模的并行处理，基于columnar data storage and column data compression 方面有优势。

# Understand Redshift  
## Open questions
- 每个node 可以有多个slice， 他们共享Node 的storage 吗？还是各自对立拥有storage？这个问题涉及到dist style == ALL 的时候的理解。
- Redshift 里面的临时表是什么东西？
## CLosed questions
- Redshift 支持partition 吗？
	- 不支持，而且不在RoadMap里。Redshift 提供解决问题的不同思路。spectrum （mainly）和 distKey 都是Redshift 提供的解决方案 
- Redshift 支持update 操作吗？
	- 支持，但不是正常的用法
- 在从dynamDB 向redshift 导入数据时，支持条件控制吗？
  - 不支持 
- 什么是sort key，distribution key？
- 什么是fact table, dimension table？
  - https://www.cnblogs.com/qlee/archive/2011/04/09/2010164.html
- cluster，node，slice 和tables 是什么关系？
- tables 之间要做join 必须在一个cluster 上吗？如果跨cluster进行table join 可行的话，distribute style 的选择怎么影响这种join？ 跨database 可以进行join 吗？
	- 这种需求是不正常的。正确的手段是 cluster->database->schema. 跨schema 是可以join 的，通过schema 来隔离业务数据。
- redshift 中的数据生命周期是怎样的？
- redshift 中的表支持索引吗？如何支持？
	- 不支持，DW 一般都是column 数据库。sort key 是一种解决方案
	- Redshift 这种数据参考通常是通过多表写入（1 Fact table, m 个Dimension table）来支持多条件的查询。

## Overview of Redshift
![enter image description here](https://docs.aws.amazon.com/redshift/latest/dg/images/02-NodeRelationships.png)
![enter image description here](https://docs.aws.amazon.com/redshift/latest/dg/images/05-InternalComponents.png)
## Concepts
- Cluster
	- 1 cluster = 1 lead node + n compute nodes
	- 1 year or 3 years reserve nodes vs hourly rate to pay
	-  1 cluster 上可以创建多个database
	- parameter groups 定义了cluster的行为 （配置文件？）
- leader Node
	- 默写SQL Functions 被设计为只能在leader node 上运行
- compute Node
	- 计算密集型和存储密集型
	- 160G---> 16T
- Node slice
	- 分配 node 的内存和存储空间  （不包括CPU？）
	- 可以看做是逻辑node 吗？
	- 提高并行能力
	- distribution key 和distribution style 的选择。
- Database
	-  1 database -----> N schemas ---------------> N*M tables
	- a schemas can be looked as a logic group.

> **计算和存储是分开的**
> **Based on PostgreSQL**
> **lead Node 相当于是client application与compute nodes 之间的代理网关**
> **只有当有两个以上的compute nodes 才需要 lead Node**
> **Node 之间的通信是专用的快速网络，与client applications 访问redshift 的通信网络是隔离的**
> **一个cluster 上可以有多个database**
> **redshift 支持OLTP 能力**



# Best Design principle of table

## Do you need a sort key
![enter image description here](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2016/12/05/o_redshift_tables_3_1.gif)

choosing sort keys, distribution styles, compression encodings

## choose the best sort key
sort key 是最主要的帮助 zone map 高效的手段
1. 每个表可以有一个sort key, 每个表可以有一个或多个列被声明为sort key
2. 通常时间值是一个很好的sort key
3. 对于联合sort key，不要把 时间列放在第一位，而是要选择 cardinality 低的放在前面。
4. 联合sort key 的column 数量不要多于4
5. 小表（rows < 10,000） 没有必要设置sort key （因为每个记录块都的单位是1M，会记录约1million 的column 数据）

- if recently data is queried most frequently, specify the timestamp as the leading column for the sort key
	- 其实这是#2 的一个特例情况
- 对于经常进行 range filtering 和 equality filtering 的column，要把它声明为sort key
	- 因为redshift 会存续一个block 的该column 的最大，最小值，这样Redshift 可以整块跳过不符合条件的块，而不用一一比较。
- 对于经常join 的table，要把参与join 的column 声明为 both the sort key and the distribution key.
	- sort merge join is faster than hash join
	- 
## choose the best distribution style

选择distribution style 的目的在于通过在查询执行之前定位数据在哪儿来最小化redistribution 的impact
### Concepts
- **Nodes and Slices:** node has its own OS, disk, memory, 1 node can has n slices (depends on node's type)
- **Data redistribution**  
	- 数据加载到表中的时候，redshift 把表中的数据根据distribution style分布到各个slices 上 
	- 在执行 查询集合的时候，数据被物理上移动，重新 distribution 以达到性能最优
		- 例如在joining 的时候吧默写特殊的行送到指定的节点或这广播整个表到所有的节点。
	- 这种redistribution 贡献了大量的cost，并且会影响整个系统的性能
	- join and aggregation 是产生 redistribution 之源
- **Data distribution goals**
	- 均匀地分配workload 到cluster 所有的nodes
	- 在查询执行中最小化数据移动
### 3 Distribution styles
- **EVEN**  
	- round-robin fashion distributes to all slices
	- 适用场景
		- table 不参与join
		- 没有对KEY 和ALL 的明显需求
- **KEY**
	- 基于某一列的值来分配记录，尽量把match 的值放在相同的node slice。
	- 对于一对儿join的表，join 列 尽量保证match 的数据被物理的保存在一起。
	- 时间变量非常**不适合**作为KEY
- **ALL**
	- small table （< 3Million）
	- 整个表被分别在每一个节点（每个Node 的第一个Slice）
	- N 倍放大存储，load，update，insert 需要更多的时间
	- 适用场景
		- 相对慢的移动table，表的update 不频繁，不多
		- Small dimension table 不会从中显著获益，因为它们本身 redistribution 的cost 就很低。

> 在表上声明distribution style，在cluster level 来处理 数据distribution。不支持在database 上的partition 概念。

### Designating Distribution Styles
**Data Warehouse Concepts**
- Star schema https://en.wikipedia.org/wiki/Star_schema
- Fact table： https://en.wikipedia.org/wiki/Fact_table
- Dimension table: https://en.wikipedia.org/wiki/Dimension_(data_warehouse)

**Principles**
1. **为所有的表声明主外键**
2. **Distribute the fact table and its largest dimension table on their common columns.**
	- Designate both the dimension table's primary key and the fact table's corresponding foreign key as DISTKEY
3. **Designate distribution keys for the other dimension tables.**
4. **Evaluate whether to change some of the dimension tables to use ALL distribution.**
5. **Use EVEN distribution for the remaining tables.**

> You cannot change the distribution style of a table after it is created. To use a different distribution style, you can recreate the table and populate the new table with a deep copy.


> 可以在一个表的数据上创建新表，为新表选择不同的distribution key。这样的方式来适应不同的查询需求  https://docs.aws.amazon.com/redshift/latest/dg/c_Distribution_examples.html
> - 如果表的数据总是在增加，新表的数据会随之变化吗？
>      - suppose是不会的。
>      - 一些不大的表可以用这种策略？
## choose a column compression type
- COPY command 会自动选择合适的compression type (**Recommended**)
- 可以为每一列设置自己的encoding type
- 不可以在表创建后改变encoding type，可以在新加入表的列声明encoding type
- **sort keys 采用 raw encoding， i.e. uncompressed form**

## Redshift 参考实例

http://www.raincent.com/content-85-8063-1.html Yelp 的使用模式，很值得一看。

[Best Practices For High-Performance ETL To Redshift.pdf - Google Drive](https://drive.google.com/file/d/1DV8LI1qfvWfcz584LaLoBlbt9cvpc-JD/view "Best Practices For High-Performance ETL To Redshift.pdf - Google Drive")

[Redshift Sort Keys - Choosing Best Sort Key | Hevo Blog](https://hevodata.com/blog/redshift-sort-keys-choosing-best-sort-style/ "Redshift Sort Keys - Choosing Best Sort Key | Hevo Blog")


https://aws.amazon.com/blogs/big-data/amazon-redshift-engineerings-advanced-table-design-playbook-compound-and-interleaved-sort-keys/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODA3NzgxOTQsMzg3NDQwODYxLDExOT
Q3NDUwNDIsLTEzNTkwODEzMDgsLTIxMDQwMzI5NzAsLTE3Mzkz
ODgxNjAsLTIxMDA1MTMxMzNdfQ==
-->