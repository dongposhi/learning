

# Open issues
## 如果一个表创建了一个LSI，该LSI 的索引结构设计为离散度不高的字段（也就是说对于每个索引可能会有很多记录  >10000）。这个时候针对该索引的一次查询会消耗多少read capactity？

# FAQ
## HASH key + SORT key 必须保证唯一吗？
**A:** 是的
##  Index 必须保证唯一吗？
**A:** 不需要
## sort key 支持复杂的查询条件吗？例如 like？
**A:** 不支持，最多支持between 和 startWith
## 如何计算read capacity unit？
**A：** 每个Item （size < 4K） 记为一个RCU。 一个item （size > 4) 的RCU = size / 4K + 1
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMxNjgxNTg4OCwtMTIyNDIzMDYwM119
-->