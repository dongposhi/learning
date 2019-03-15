

## Scala 特性
Scala 支持了很多语法糖让整个语言追求快速而不是工程化。他是一个复杂的语言，是聪明人的玩具。

### 快速了解Scala 语法特性
https://www.cnblogs.com/ahu-lichang/p/7207847.html

### 为什么scala 引入伴生类和伴生对象的概念？
1. 伴生类和伴生对象共有相同的名字。 class A，object A
2. Scala 的设计者希望设计一个纯OO 的语言，而static，primitive 是他希望消灭的（**we wanted to be a pure object-oriented language, where every value is an object, every operation is a method call**）。而Scala 是在JVM 之上构建的。他又需要和Java（static，primitive）打交道。这就是伴生方式设计的缘起。
3. https://stackoverflow.com/questions/7302206/why-doesnt-scala-have-static-members-inside-a-class
4. **Companion objects also have the benefit of being extensible, ie. take advantage of inheritance and mixin composition (separate from emulating static functionality for interop).**

### Scala 中的Mixin 设计  ------ Keyword “with”
https://docs.scala-lang.org/tour/mixin-class-composition.html


### Scala 的 case class
case class 是一个毁誉参半的设计。这个语法糖
	一方面有类似于lombok 的@Data 效果。
	一方面是为了更好的支持pattern case
	也有一些不可变数据structure 的考量（？实际是可变的）

### sealed 
sealed 修饰的trait, class 的派生类只能在同一文件中
sealed 关键字增加了 pattern case 的编译器完备性检查

### _
_ 代表了一种lambda 的变量简写方式
``` scala
 x => sum( a+x)  
equals
 sum(a+_)
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3MzUzMDAxMCwxNDEwNTcxNjcyLDg5MD
kwOTk5MV19
-->