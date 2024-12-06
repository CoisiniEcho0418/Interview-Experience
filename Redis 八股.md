# Redis 八股

------

## Redis如何为集合中的每个元素独立设置过期时间

[Redis如何为 List/Set/Hash 的元素设置单独的过期时间-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2370819)



## Redis Sorted Zet数据结构

[Redis Sorted Set 底层实现原理深度解读与排行榜实战 (qq.com)](https://mp.weixin.qq.com/s/xwmQyhMEpHFD7g2V1JsoSg)

[redis-sorted set(zset)实现_sortedset和zset-CSDN博客](https://blog.csdn.net/swl1993831/article/details/105348485?ops_request_misc=%7B%22request%5Fid%22%3A%22E41AEF44-EB0D-4726-8C23-824F4163510F%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=E41AEF44-EB0D-4726-8C23-824F4163510F&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-105348485-null-null.142^v100^pc_search_result_base2&utm_term=redis sortedzet数据结构&spm=1018.2226.3001.4187)



## 如何基于Redis实现分布式锁？

[支付宝一面：如何基于Redis实现分布式锁？](https://mp.weixin.qq.com/s/y8kAflIenRpa4zIwCP_8iA)

### Redis 如何解决集群情况下分布式锁的可靠性？

为了避免单点故障，生产环境下的 Redis 服务通常是集群化部署的。

Redis 集群下，上面介绍到的分布式锁的实现会存在一些问题。由于 Redis 集群数据同步到各个节点时是异步的，如果在 Redis 主节点获取到锁后，在没有同步到其他节点时，Redis 主节点宕机了，此时新的 Redis 主节点依然可以获取锁，所以多个应用服务就可以同时获取到锁。

<img src="https://mmbiz.qpic.cn/mmbiz_png/iaIdQfEric9TwLNZIGgTb8Ywu79icHWJGFOne9WNlPDR8hVPibvqSumwtXshfNkQI2FL30uDUGfKiaE10viaTwUfVmQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:67%;" />

针对这个问题，Redis 之父 antirez 设计了 **Redlock 算法**[3] 来解决。

<img src="https://mmbiz.qpic.cn/mmbiz_png/iaIdQfEric9TwLNZIGgTb8Ywu79icHWJGFOZibRKz6ZYoP5j0vmdjxH4zOONNF1cIPiazFFOKjzDicpLHgMDUVE5j2qg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom:50%;" />

Redlock 算法的思想是让客户端向 Redis 集群中的多个独立的 Redis 实例依次请求申请加锁，如果客户端能够和半数以上的实例成功地完成加锁操作，那么我们就认为，客户端成功地获得分布式锁，否则加锁失败。

即使部分 Redis 节点出现问题，只要保证 Redis 集群中有半数以上的 Redis 节点可用，分布式锁服务就是正常的。

Redlock 是直接操作 Redis 节点的，并不是通过 Redis 集群操作的，这样才可以避免 Redis 集群主从切换导致的锁丢失问题。

Redlock 实现比较复杂，性能比较差，发生时钟变迁的情况下还存在安全性隐患。《数据密集型应用系统设计》一书的作者 Martin Kleppmann 曾经专门发文（**How to do distributed locking - Martin Kleppmann - 2016**[4]）怼过 Redlock，他认为这是一个很差的分布式锁实现。感兴趣的朋友可以看看[Redis 锁从面试连环炮聊到神仙打架](https://mp.weixin.qq.com/s?__biz=Mzg3NjU3NTkwMQ==&mid=2247505097&idx=1&sn=5c03cb769c4458350f4d4a321ad51f5a&source=41&scene=21#wechat_redirect)这篇文章，有详细介绍到 antirez 和 Martin Kleppmann 关于 Redlock 的激烈辩论。

实际项目中不建议使用 Redlock 算法，成本和收益不成正比。

如果不是非要实现绝对可靠的分布式锁的话，其实单机版 Redis 就完全够了，实现简单，性能也非常高。如果你必须要实现一个绝对可靠的分布式锁的话，可以基于 ZooKeeper 来做，只是性能会差一些。



## Redisson 分布式锁使用注意事项以及内部原理

[分布式锁实现，Redisson篇 (qq.com)](https://mp.weixin.qq.com/s/1JEpR6rQTPrZBsSPBNol2w)

[【SpringBoot DB 系列】redisson 分布式锁使用及注意事项 (qq.com)](https://mp.weixin.qq.com/s/EZu0HhEEs9DepQEfzMhHdA)

[还不懂如何实现分布式锁？使用Redisson，一行代码搞定！](https://mp.weixin.qq.com/s/D7gaiodEY1a1yx3tFlNiuw)



## Redis实现限流

[太优雅了！用Redis高效实现限流功能! (qq.com)](https://mp.weixin.qq.com/s/i0aDxP94oEMuw_IKyRD2mg)



## 布隆过滤器

[布隆(Bloom Filter)过滤器——全面讲解，建议收藏-CSDN博客](https://blog.csdn.net/qq_41125219/article/details/119982158)



## 数据库和Redis缓存如何保证一致性

[数据库和缓存如何保证一致性？ | 小林coding](https://xiaolincoding.com/redis/architecture/mysql_redis_consistency.html)



## Cache Aside策略下如何保证两个操作都能执行成功？

应用要把数据 X 的值从 1 更新为 2，先成功更新了数据库，然后在 Redis 缓存中删除 X 的缓存，但是这个操作却失败了，这个时候数据库中 X 的新值为 2，Redis 中的 X 的缓存值为 1，出现了数据库和缓存数据不一致的问题。

![图片](https://cdn.xiaolincoding.com//mysql/other/2a2ea2854bbc3ae8ae86d7da45fa32ee.png)

那么，后续有访问数据 X 的请求，会先在 Redis 中查询，因为缓存并没有 诶删除，所以会缓存命中，但是读到的却是旧值 1。

其实不管是先操作数据库，还是先操作缓存，只要第二个操作失败都会出现数据一致的问题。

问题原因知道了，该怎么解决呢？有两种方法：

- 消息队列重试机制。
- 订阅 MySQL binlog，再操作缓存。

先来说第一种。

### [#](https://xiaolincoding.com/redis/architecture/mysql_redis_consistency.html#消息队列重试机制)消息队列重试机制

我们可以引入**消息队列**，将第二个操作（删除缓存）要操作的数据加入到消息队列，由消费者来操作数据。

- 如果应用**删除缓存失败**，可以从消息队列中重新读取数据，然后再次删除缓存，这个就是**重试机制**。当然，如果重试超过的一定次数，还是没有成功，我们就需要向业务层发送报错信息了。
- 如果**删除缓存成功**，就要把数据从消息队列中移除，避免重复操作，否则就继续重试。

举个例子，来说明重试机制的过程。

![图片](https://cdn.xiaolincoding.com//mysql/other/a4440f0d572612e0832b903e4a62bd2b.png)

这个方案缺点是，对代码入侵性比较强，因为需要改造原本业务的代码。

### [#](https://xiaolincoding.com/redis/architecture/mysql_redis_consistency.html#订阅-mysql-binlog-再操作缓存)订阅 MySQL binlog，再操作缓存

「**先更新数据库，再删缓存**」的策略的第一步是更新数据库，那么更新数据库成功，就会产生一条变更日志，记录在 binlog 里。

于是我们就可以通过订阅 binlog 日志，拿到具体要操作的数据，然后再执行缓存删除，阿里巴巴开源的 Canal 中间件就是基于这个实现的。

Canal 模拟 MySQL 主从复制的交互协议，把自己伪装成一个 MySQL 的从节点，向 MySQL 主节点发送 dump 请求，MySQL 收到请求后，就会开始推送 Binlog 给 Canal，Canal 解析 Binlog 字节流之后，转换为便于读取的结构化数据，供下游程序订阅使用。

下图是 Canal 的工作原理：

![图片](https://cdn.xiaolincoding.com//mysql/other/2ee2280e9f59b6b4879ebdec6eb0cf52.png)

前面我们说到直接用消息队列重试机制方案的话，会对代码造成入侵，那么 Canal 方案就能很好的规避这个问题，因为它是直接订阅 binlog 日志的，和业务代码没有藕合关系，因此我们可以通过 Canal+ 消息队列的方案来保证数据缓存的一致性。

具体的做法是：**将binlog日志采集发送到MQ队列里面，然后编写一个简单的缓存删除消息者订阅binlog日志，根据更新log删除缓存，并且通过ACK机制确认处理这条更新log，保证数据缓存一致性**

这里有一个很关键的点，**必须是删除缓存成功，再回 ack 机制给消息队列**，否则可能会造成消息丢失的问题，比如消费服务从消息队列拿到事件之后，直接回了 ack，然后再执行删除缓存操作的话，如果删除缓存的操作还是失败了，那么因为提前给消息队列回 ack了，就没办重试了。

所以，如果要想保证「先更新数据库，再删缓存」策略第二个操作能执行成功，我们可以使用：

- 消息队列来重试缓存的删除，优点是保证缓存一致性的问题，缺点会对业务代码入侵
- 订阅 MySQL binlog + 消息队列 + 重试缓存的删除，优点是规避了代码入侵问题，也很好的保证缓存一致性的问题，缺点就是引入的组件比较多，对团队的运维能力比较有高要求。

这两种方法有一个共同的特点，都是采用**异步操作缓存**。



## Redis和本地缓存的区别和对比

![image-20241014212634373](assets\image-20241014212634373.png)



## Redis使用场景

Redis 除了可以用来缓存高频访问的数据之外，还以用来实现：

- **分布式锁**：通过 Redis 来做分布式锁是一种比较常见的方式。通常情况下，我们都是基于 Redisson 来实现分布式锁。关于 Redis 实现分布式锁的详细介绍，可以看我写的这篇文章：[如何基于 Redis 实现分布式锁？](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247534440&idx=1&sn=96a4de944bd62b59fc4c033f93a89bf4&scene=21#wechat_redirect)。
- **限流**：一般是通过 Redis + Lua 脚本的方式来实现限流。如果不想自己写 Lua 脚本的话，也可以直接利用 Redisson 中的 `RRateLimiter` 来实现分布式限流，其底层实现就是基于 Lua 代码+令牌桶算法。
- **消息队列**：Redis 自带的 List 数据结构可以作为一个简单的队列使用。Redis 5.0 中增加的 Stream 类型的数据结构更加适合用来做消息队列。它比较类似于 Kafka，有主题和消费组的概念，支持消息持久化以及 ACK 机制。
- **延时队列**：Redisson 内置了延时队列（基于 Sorted Set 实现的）。
- **分布式 Session** ：利用 String 或者 Hash 数据类型保存 Session 数据，所有的服务器都可以访问。
- **复杂业务场景**：通过 Redis 以及 Redis 扩展（比如 Redisson）提供的数据结构，我们可以很方便地完成很多复杂的业务场景比如通过 Bitmap 统计活跃用户、通过 Sorted Set 维护排行榜。



## [从一个事故中理解Redis（几乎）所有知识点](https://mp.weixin.qq.com/s/39Q5-vvIBmlmRVW8tzdjyA)