


Shuffle描述着数据从map task输出到reduce task输入的这段过程。在分布式情况下，reduce task需要跨节点去拉取其它节点上的map task结果。这一过程将会产生网络资源消耗和内存，磁盘IO的消耗。


在Map阶段，k-v溢写时，采用的正是快排；而溢出文件的合并使用的则是归并；在Reduce阶段，通过shuffle从Map获取的文件进行合并的时候采用的也是归并；最后阶段则使用了堆排作最后的合并过程。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMDQ5NTc4OTFdfQ==
-->