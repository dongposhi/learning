


HystrixCommand
* 这个类是Hystrix 库对外暴露的主要接口。通过开放该类给客户集成来形成对RPC 的wrapper，从而实现对限流，熔断。类似于拦截
* 所以该类唯一的abstract method是 run()
* 实现了HystrixExecutable 接口，提供了blocking 调用方式的实现
	* queue
	* execute
* 实现了AbstractCommand 的抽象函数




![enter image description here](http://www.plantuml.com/plantuml/png/XLBHRi8m37ptL_G7-Wz0XOGqG0Zs0y7r8AADmN4cAhH_NpP08gM4jvplT8hlFF9gH4FR061Zl2zcdLUnvyeA1giJ8NCWIRdFVPpMA-RcrVo3kfuTjwrnzKhuIAAM_22ze0Xmc7koNDCfaDVAu9u6cJdlqldqcvmkMUsNxQzKiSCWGWwxZOfHsWqlC3qJ1hNiPim7033Ro1hKuYeUt-1DVO6omEjpez4qGltFw8xeRhSc7ng57u2UHS5b-QAJZFAXePnVcdf6cMJzNlUNV4QLzf3zu7Chx_VfTmGZSdMyy5y0 "AbstractCommand impl class diagram-9819f541__90999z82xeu4hXtks")










<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxMzY0MzMyNywtMTU5NzM2MzYzMiwxND
YxMzQ3NTIyLDEwNDIwNjAyODAsMzI1NjAzNjQyLDEzMjczOTg1
MDFdfQ==
-->