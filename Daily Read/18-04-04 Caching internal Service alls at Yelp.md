


# Caching Internal Service Calls at Yelp

https://engineeringblog.yelp.com/2018/03/caching-internal-service-calls-at-yelp.html

[Casper](https://github.com/Yelp/casper): caching Proxy of Yelp for internal **services** traffic flowomg

based on Nginx and OpenResty ( 基于Nginx + Lua 的web 平台）

解决什么问题： 在SOA 大规模部署后带来的问题。

Yelp 的后台演进路线  （ 规模提升带来的必然）
	memcached ---------> SOA
      数据共享--------------> 服务共享

**描述了memcache 在大规模场景下的局限性**

基于问题总结出来了几个指导性的原则：
- **transparency**
- **built-in operational metrics** 
- **isolation from app deploys**


Yelp service 结构

PaaSTA  relied on  SmartStack （Nerve and Synapse）提供服务发现和路由
![enter image description here](https://engineeringblog.yelp.com/images/posts/2018-03-12-caching-internal-service-calls-at-yelp/yelp_service_call.png)


## Casper 在 cache 上的选型
1. 在模型验证中得到了认知： cache 服务要与Casper 本身分开部署。这样Casper 的重启不会带来cache 的重启。casper 运行在docker container 上。
2. Cassandra 代替了Redis。 主要的原因在于Redis cluster 在当时没有合适的Lua driver
3. Cassandra 采用TMPFS （i.e. In-memory Cassandra）提供了媲美Redis 的性能。
## Cache key 调优
1. 基于整个request 作为 cache key 是不可选的。因为headers 是per request 不同的。
2. URI + method  （第一版）不使用于 http gzip-enable service
3. final decsion： developer configurae to optionally include http headers in the cache key   
4. 相关文章： https://www.fastly.com/blog/best-practices-using-vary-header/

## Caching bulk endpoint 所遇到的有趣的问题
1. bulk查询参数顺序变化不应该影响命中，但是由于参数变化导致cache key 不一致影响了命中
2. 如果多次查询 中包含了相同的子集，但是由于key 的问题，无法利用到Cache 中子集部分。
3. 如果一个bulk 查询中的子集变化，cache 会将所有包含该子集的cache 都过期处理，导致了浪费。

**Solution**： 基于请求中resourceIds 的组织特点（即逗号分割）以及response 中JSON based 对象数组的组织方式。在Request， response 的 中间做了一个夹层（拦截）来解决这个问题。**Casper’s bulk endpoint logic relies on assumptions specific to Swagger and JSON-encoding.**

## Casper 是一个服务调用的缓存方案。在 Yelp 内部开放一个服务，可以通过配置的方式来声明是否使用Casper。这点类似与Amazon 的Cache out ？


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEzMDc3MzA3NiwtMTU1ODcxNjA0MF19
-->