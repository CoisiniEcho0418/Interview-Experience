# Redis

------

## Redis 数据类型

![image-20240814213808140](assets\image-20240814213808140.png)



## Redis 通用命令

![image-20240814214808465](assets\image-20240814214808465.png)



## Redis 基础介绍

### Key 结构

![image-20240814215746419](assets\image-20240814215746419.png)



### String类型

![image-20240814214922793](assets\image-20240814214922793.png)

![image-20240814215003408](assets\image-20240814215003408.png)

![image-20240814220004962](assets\image-20240814220004962.png)



### Hash 类型

#### 数据结构

![image-20240815110727460](assets\image-20240815110727460.png)



#### 操作命令

![image-20240815110747627](assets\image-20240815110747627.png)



### List 类型

#### 数据结构

![image-20240815111343788](assets\image-20240815111343788.png)

#### 操作命令

![image-20240815111537474](assets\image-20240815111537474.png)



### SET 类型

#### 数据结构

![image-20240815112159778](assets\image-20240815112159778.png)

#### 操作命令

![image-20240815112507826](assets\image-20240815112507826.png)



### SortedSet 类型

#### 数据结构

![image-20240815112840545](assets\image-20240815112840545.png)

#### 操作命令

![image-20240815113116960](assets\image-20240815113116960.png)



## Redis 客户端

![image-20240815114118052](assets\image-20240815114118052.png)

### Jedis

![image-20240815114543220](assets\image-20240815114543220.png)

![image-20240815114708812](assets\image-20240815114708812.png)

![image-20240815115201680](assets\image-20240815115201680.png)



### SpringDataRedis

![image-20240815115825133](assets\image-20240815115825133.png)

![image-20240815120211505](assets\image-20240815120211505.png)



![image-20240815120343757](assets\image-20240815120343757.png)

![image-20240815120558597](assets\image-20240815120558597.png)

![image-20240815120618472](assets\image-20240815120618472.png)

![image-20240815121008817](assets\image-20240815121008817.png)

![image-20240815121416858](assets\image-20240815121416858.png)

![image-20240815121439319](assets\image-20240815121439319.png)

![image-20240815122326330](assets\image-20240815122326330.png)

![image-20240815122608947](assets\image-20240815122608947.png)

![image-20240815122643869](assets\image-20240815122643869.png)

![image-20240815123033071](assets\image-20240815123033071.png)



## Redis 实战篇

![image-20240815160633982](assets\image-20240815160633982.png)

### 数据表

![image-20240815160808460](assets\image-20240815160808460.png)

### 短信登录

![image-20240815164405924](assets\image-20240815164405924.png)

### 登录校验

![image-20240815202211835](assets\image-20240815202211835.png)

![image-20240815202334461](assets\image-20240815202334461.png)

### Session共享问题

![image-20240815203705779](assets\image-20240815203705779.png)

![image-20240817231253903](assets\image-20240817231253903.png)

![image-20240816195941573](assets\image-20240816195941573.png)

![image-20240816195620753](assets\image-20240816195620753.png)

![image-20240816201908943](assets\image-20240816201908943.png)

### 登录拦截优化

![image-20240816214515153](assets\image-20240816214515153.png)

### 商户查询缓存

![image-20240816215229359](assets\image-20240816215229359.png)

![image-20240816215356820](assets\image-20240816215356820.png)

![image-20240816220432071](assets\image-20240816220432071.png)

### 缓存更新策略

![image-20240816222802457](assets\image-20240816222802457.png)

![image-20240816222939982](assets\image-20240816222939982.png)

![image-20240816223323048](assets\image-20240816223323048.png)

![image-20240816223735421](assets\image-20240816223735421.png)

![image-20240816223755672](assets\image-20240816223755672.png)

### 缓存穿透

![image-20240816224910740](assets\image-20240816224910740.png)

![image-20240818200803610](assets\image-20240818200803610.png)

![image-20240818202746753](assets\image-20240818202746753.png)

### 缓存雪崩

![image-20240818204544330](assets\image-20240818204544330.png)

### 缓存击穿

![image-20240818204919426](assets\image-20240818204919426.png)

![image-20240818205318320](assets\image-20240818205318320.png)

![image-20240818205338297](assets\image-20240818205338297.png)

#### 互斥锁解决方案

![image-20240818205911418](assets\image-20240818205911418.png)

#### 逻辑过期解决方案

![image-20240818222609966](assets\image-20240818222609966.png)

![image-20240819010759880](assets\image-20240819010759880.png)



### 优惠券秒杀

#### 全局ID生成器

![image-20240819213005120](assets\image-20240819213005120.png)

![image-20240819213223232](assets\image-20240819213223232.png)

![image-20240819213348413](assets\image-20240819213348413.png)

![image-20240819214843271](assets\image-20240819214843271.png)

#### 优惠券表信息

![image-20240819215402296](assets\image-20240819215402296.png)

![image-20240819215245556](assets\image-20240819215245556.png)

![image-20240819215338747](assets\image-20240819215338747.png)

#### 添加优惠券

![image-20240819215522963](assets\image-20240819215522963.png)

![image-20240819215753352](assets\image-20240819215753352.png)

#### 实现优惠券秒杀下单

![image-20240822153126447](assets\image-20240822153126447.png)

#### 超卖问题

![image-20240822164953295](assets\image-20240822164953295.png)

##### 悲观锁和乐观锁

![image-20240822165149883](assets\image-20240822165149883.png)

![image-20240822165455683](assets\image-20240822165455683.png)

![image-20240822165548960](assets\image-20240822165548960.png)

![image-20240822170313007](assets\image-20240822170313007.png)

> **Tips：分段锁可以解决乐观锁成功率低的问题**

#### 一人一单

![image-20240822170615616](assets\image-20240822170615616.png)

![image-20240822215852308](assets\image-20240822215852308.png)

![image-20240822221311138](assets\image-20240822221311138.png)

#### 分布式锁

![image-20240822230041299](assets\image-20240822230041299.png)

![image-20240822230242347](assets\image-20240822230242347.png)



##### 实现分布式锁的三种方法

![image-20240822230828241](assets\image-20240822230828241.png)



##### 基于Redis实现分布式锁

![image-20240823031933102](assets\image-20240823031933102.png)

![image-20240823032058526](assets\image-20240823032058526.png)

**版本一存在的问题及解决方案：**

![image-20240823033248646](assets\image-20240823033248646.png)

![image-20240823033346209](assets\image-20240823033346209.png)

![image-20240823033406008](assets\image-20240823033406008.png)

![image-20240823033942193](assets\image-20240823033942193.png)

##### 分布式锁的原子性

> **判断锁标识和删除锁操作要保证原子性**

![image-20240823162644068](assets\image-20240823162644068.png)

##### Lua 脚本

![image-20240823165310854](assets\image-20240823165310854.png)

![image-20240823165758640](assets\image-20240823165758640.png)

![image-20240823170320378](assets\image-20240823170320378.png)

![image-20240823171757443](assets\image-20240823171757443.png)

##### Redisson（在Redis基础上实现的分布式工具集合）

![image-20240823190207677](assets\image-20240823190207677.png)

![image-20240823190954377](assets\image-20240823190954377.png)

![image-20240823191136405](assets\image-20240823191136405.png)

###### Redisson可重入锁原理（利用lua脚本--保证原子性）

![image-20240823194235153](assets\image-20240823194235153.png)

![image-20240823194442376](assets\image-20240823194442376.png)

###### Redisson分布式锁原理（可重试【信号量，消息订阅】&超时自动续约【定时任务】）

> Redisson 分布式锁 解决 “不可重试” 和 “超时释放” 问题的方法，如下图（其中 watchDog是一种设置定时任务的思路，每次过了 expire time 的三分之一之后就会开始刷新 expire time，一直递归循环调用，直至 unlock 后被cancel）：

![image-20240823211841418](assets\image-20240823211841418.png)

> 下图中的PubSub 指的是 publish 和 subscribe，即“发布-订阅”机制

![image-20240823212358873](assets\image-20240823212358873.png)

###### Redisson 的 multiLock 原理

> 主从一致性问题产生的原因

![image-20240823213020901](assets\image-20240823213020901.png)

![image-20240823213333638](assets\image-20240823213333638.png)

![image-20240823214544317](assets\image-20240823214544317.png)

#### Redis 优化秒杀 - 异步

![image-20240824014305026](assets\image-20240824014305026.png)

![image-20240824014729736](assets\image-20240824014729736.png)

![image-20240824034217951](assets\image-20240824034217951.png)

![image-20240824034408151](assets\image-20240824034408151.png)

#### Redis 消息队列实现异步秒杀

![image-20240824042516151](assets\image-20240824042516151.png)

![image-20240824042545962](assets\image-20240824042545962.png)

##### 基于 List 结构模拟消息队列

![image-20240824042748467](assets\image-20240824042748467.png)

![image-20240824043031719](assets\image-20240824043031719.png)

##### 基于 PubSub（发布订阅）的消息队列

![image-20240824043342443](assets\image-20240824043342443.png)

![image-20240824043631643](assets\image-20240824043631643.png)

##### 基于 stream 的消息队列

![image-20240824043942020](assets\image-20240824043942020.png)

![image-20240824044233864](assets\image-20240824044233864.png)

![image-20240824044332244](assets\image-20240824044332244.png)

![image-20240824044454548](assets\image-20240824044454548.png)

##### 消费者组

![image-20240824163323625](assets\image-20240824163323625.png)

![image-20240824163521344](assets\image-20240824163521344.png)

![image-20240824163855401](assets\image-20240824163855401.png)

###### XACK 用法

![image-20240824164212608](assets\image-20240824164212608.png)



###### XPENDING - 查看pending list

![image-20240824164620798](assets\image-20240824164620798.png)

![image-20240824164659387](assets\image-20240824164659387.png)

![image-20240824165205562](assets\image-20240824165205562.png)

![image-20240824165348024](assets\image-20240824165348024.png)

![image-20240824165736480](assets\image-20240824165736480.png)

![image-20240824165941664](assets\image-20240824165941664.png)

### 达人探店

![image-20240824204630036](assets\image-20240824204630036.png)

![image-20240824223638005](assets\image-20240824223638005.png)

### 好友关注

![image-20240825005123606](assets\image-20240825005123606.png)

![image-20240825005253978](assets\image-20240825005253978.png)

![image-20240825012115643](assets\image-20240825012115643.png)



### 关注推送——Feed流

![image-20240825021533433](assets\image-20240825021533433.png)

![image-20240825021636070](assets\image-20240825021636070.png)

![image-20240825021657256](assets\image-20240825021657256.png)

![image-20240825021721085](assets\image-20240825021721085.png)

#### 拉模式（读扩散）

![image-20240825021851883](assets\image-20240825021851883.png)

#### 推模式（写扩散）

![image-20240825022028827](assets\image-20240825022028827.png)

#### 推拉结合（读写混合）

![image-20240825022303401](assets\image-20240825022303401.png)

![image-20240825022424724](assets\image-20240825022424724.png)

#### 基于推模式实现关注推送功能

![image-20240825022751868](assets\image-20240825022751868.png)

###### 滚动分页

![image-20240825023024937](assets\image-20240825023024937.png)

![image-20240825023150215](assets\image-20240825023150215.png)

![image-20240825031017920](assets\image-20240825031017920.png)

![image-20240825031524349](assets\image-20240825031524349.png)

![image-20240825031603929](assets\image-20240825031603929.png)

### 附近商铺

![image-20240825040640312](assets\image-20240825040640312.png)

![image-20240825042234424](assets\image-20240825042234424.png)

![image-20240825042543856](assets\image-20240825042543856.png)

![image-20240825044706979](assets\image-20240825044706979.png)



### 用户签到

![image-20240825184934714](assets\image-20240825184934714.png)

![image-20240825191113951](assets\image-20240825191113951.png)

![image-20240825191825862](assets\image-20240825191825862.png)

![image-20240825192026303](assets\image-20240825192026303.png)

![image-20240825202932037](assets\image-20240825202932037.png)

![image-20240825202955030](assets\image-20240825202955030.png)

### UV统计

![image-20240825211211589](assets\image-20240825211211589.png)

![image-20240825220714680](assets\image-20240825220714680.png)



## Redis 高级篇

![image-20240825222224146](assets\image-20240825222224146.png)

### Redis 持久化

#### RDB

![image-20240825222514128](assets\image-20240825222514128.png)

![image-20240825222820908](assets\image-20240825222820908.png)

![image-20240825223558947](assets\image-20240825223558947.png)

![image-20240825223719212](assets\image-20240825223719212.png)

#### AOF

![image-20240825223833185](assets\image-20240825223833185.png)

![image-20240825224105143](assets\image-20240825224105143.png)

![image-20240825224651166](assets\image-20240825224651166.png)

![image-20240825231012416](assets\image-20240825231012416.png)



### Redis主从

> Redis集群（Windows）参考文章：[Windows 系统下本地单机搭建 Redis 主从 + 哨兵 + 集群（一主二从三哨兵六-八集群）_windows redis集群搭建一主两从-CSDN博客](https://blog.csdn.net/weixin_42722953/article/details/135086396?ops_request_misc=%7B%22request%5Fid%22%3A%22172460840516800213071206%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=172460840516800213071206&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~times_rank-5-135086396-null-null.142^v100^pc_search_result_base2&utm_term=windows下运行多个redis 哨兵&spm=1018.2226.3001.4187)

#### 搭建主从架构

![image-20240826003323982](assets\image-20240826003323982.png)

![image-20240826010607022](assets\image-20240826010607022.png)

![image-20240826010833761](assets\image-20240826010833761.png)

![image-20240826010742960](assets\image-20240826010742960.png)



#### 数据同步原理

##### 全量同步

![image-20240826011745211](assets\image-20240826011745211.png)

![image-20240826011952114](assets\image-20240826011952114.png)

![image-20240826012045771](assets\image-20240826012045771.png)



##### 增量同步

![image-20240826012925884](assets\image-20240826012925884.png)

![image-20240826013142495](assets\image-20240826013142495.png)

![image-20240826013214872](assets\image-20240826013214872.png)



### Redis 哨兵

![image-20240826013610416](assets\image-20240826013610416.png)

![image-20240826013715374](assets\image-20240826013715374.png)

![image-20240826013850809](assets\image-20240826013850809.png)

![image-20240826013943691](assets\image-20240826013943691.png)

![image-20240826014022937](assets\image-20240826014022937.png)

![image-20240826021458049](assets\image-20240826021458049.png)

![image-20240826021538716](assets\image-20240826021538716.png)

![image-20240826021643395](assets\image-20240826021643395.png)



### Redis 分片集群（TODO，待实践）

![image-20240826030450195](assets\image-20240826030450195.png)

#### 散列插槽

![image-20240826031608025](assets\image-20240826031608025.png)

![image-20240826031734622](assets\image-20240826031734622.png)

![image-20240826031913432](assets\image-20240826031913432.png)



#### 集群伸缩

![image-20240826032126918](assets\image-20240826032126918.png)

![image-20240826032542632](assets\image-20240826032542632.png)

#### 故障转移

![image-20240826032850246](assets\image-20240826032850246.png)

![image-20240826033135112](assets\image-20240826033135112.png)

![image-20240826033218057](assets\image-20240826033218057.png)

#### RedisTemplate 访问分片集群

![image-20240826033313228](assets\image-20240826033313228.png)



### 多级缓存

![image-20240826033746997](assets\image-20240826033746997.png)

![image-20240826034003558](assets\image-20240826034003558.png)

![image-20240826034115624](assets\image-20240826034115624.png)

#### JVM 缓存

![image-20240827002508958](assets\image-20240827002508958.png)

![image-20240827002608341](assets\image-20240827002608341.png)

![image-20240827002853475](assets\image-20240827002853475.png)

![image-20240827003656211](assets\image-20240827003656211.png)

![image-20240827004053492](assets\image-20240827004053492.png)

#### Lua 语法

![image-20240827004542903](assets\image-20240827004542903.png)

![image-20240827004712795](assets\image-20240827004712795.png)

![image-20240827004856273](assets\image-20240827004856273.png)

![image-20240827005030651](assets\image-20240827005030651.png)

![image-20240827005148721](assets\image-20240827005148721.png)

![image-20240827005352172](assets\image-20240827005352172.png)

![image-20240827005629056](assets\image-20240827005629056.png)

![image-20240827005838452](assets\image-20240827005838452.png)

![image-20240827010027550](assets\image-20240827010027550.png)



#### OpenResty

![image-20240827010504855](assets\image-20240827010504855.png)

![image-20240827011338706](assets\image-20240827011338706.png)

![image-20240827011426837](assets\image-20240827011426837.png)

![image-20240827012115532](assets\image-20240827012115532.png)

![image-20240827012807979](assets\image-20240827012807979.png)

![image-20240827013018147](assets\image-20240827013018147.png)

![image-20240827013847557](assets\image-20240827013847557.png)

![image-20240827013800047](assets\image-20240827013800047.png)

![image-20240827013651617](assets\image-20240827013651617.png)

##### 根据商品id对Tomcat实现负载均衡

![image-20240827014154646](assets\image-20240827014154646.png)

> 配置反向代理集群时可以设置 hash 算法来确定某次请求指向哪台服务器，上面用的 hash 算法是根据请求的url

##### Redis 缓存预热

![image-20240827014707561](assets\image-20240827014707561.png)

![image-20240827015024723](assets\image-20240827015024723.png)

![image-20240827015411886](assets\image-20240827015411886.png)

```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
public class RedisHandler implements InitializingBean {

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;

    private static final ObjectMapper MAPPER = new ObjectMapper();

    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }

        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }

    public void saveItem(Item item) {
        try {
            String json = MAPPER.writeValueAsString(item);
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }

    public void deleteItemById(Long id) {
        redisTemplate.delete("item:id:" + id);
    }
}
```



##### OpenResty 查询 Redis

![image-20240827015634700](assets\image-20240827015634700.png)

![image-20240827015956301](assets\image-20240827015956301.png)

![image-20240827020236838](assets\image-20240827020236838.png)

![image-20240827020440158](assets\image-20240827020440158.png)

##### nginx 本地缓存

![image-20240828005337967](assets\image-20240828005337967.png)

![image-20240828005625318](assets\image-20240828005625318.png)

![image-20240828010148936](assets\image-20240828010148936.png)



#### 缓存同步（Canal）

![image-20240828010302321](assets\image-20240828010302321.png)

![image-20240828010525162](assets\image-20240828010525162.png)

![image-20240828010744798](assets\image-20240828010744798.png)

![image-20240828010814393](assets\image-20240828010814393.png)

![image-20240828011454191](assets\image-20240828011454191.png)

![image-20240828011527962](assets\image-20240828011527962.png)

![image-20240828011731158](assets\image-20240828011731158.png)

![image-20240828011844010](assets\image-20240828011844010.png)



#### 多极缓存架构总结

![image-20240828013107503](assets\image-20240828013107503.png)



### Redis最佳实践

#### redis 键值设计

##### key设计经验

![image-20240828013859409](assets\image-20240828013859409.png)

##### BigKey

![image-20240828014123493](assets\image-20240828014123493.png)

![image-20240828014354914](assets\image-20240828014354914.png)

![image-20240828015050573](assets\image-20240828015050573.png)

![image-20240828015250613](assets\image-20240828015250613.png)

##### 选择合适的数据类型（结构）

![image-20240828015544077](assets\image-20240828015544077.png)

![image-20240828015828052](assets\image-20240828015828052.png)

![image-20240828020110161](assets\image-20240828020110161.png)

![image-20240828020343433](assets\image-20240828020343433.png)

#### 批处理优化

##### pipeline（单机模式）

![image-20240828020842346](assets\image-20240828020842346.png)

![image-20240828021040969](assets\image-20240828021040969.png)

![image-20240828021206130](assets\image-20240828021206130.png)

![image-20240828021458416](assets\image-20240828021458416.png)

![image-20240828021700582](assets\image-20240828021700582.png)

##### 集群下的批处理

![image-20240828022107286](assets\image-20240828022107286.png)



#### 服务端优化

##### 持久化配置

![image-20240902234634162](assets\image-20240902234634162.png)

![image-20240902235006011](assets\image-20240902235006011.png)

##### 慢查询

![image-20240902235141499](assets\image-20240902235141499.png)

![image-20240902235254809](assets\image-20240902235254809.png)

##### 命令及安全配置

![image-20240903000227669](assets\image-20240903000227669.png)

![image-20240903000434866](assets\image-20240903000434866.png)