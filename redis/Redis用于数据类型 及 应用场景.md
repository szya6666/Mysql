### Redis用于数据类型 及 应用场景

# 字符串（String）

## 应用场景

String是最常用的一种数据类型，普通的key/ value 存储都可以归为此类.即可以完全实现目前 Memcached 的功能，并且效率更高。还可以享受Redis的定时持久化，操作日志及 Replication等功能。除了提供与 Memcached 一样的get、set、incr、decr 等操作外，Redis还提供了下面一些操作：

- 获取字符串长度
- 往字符串append内容
- 设置和获取字符串的某一段内容
- 设置及获取字符串的某一位（bit）
- 批量设置一系列字符串的内容

**应用举例**： 
统计网站访问数量、当前在线人数、微博数、粉丝数等，incr命令(++操作)

## 实现方式

String在redis内部存储默认就是一个字符串，被redisObject所引用，当遇到incr,decr等操作时会转成数值型进行计算，此时redisObject的encoding字段为int。

## 常用接口

| 接口                             | 介绍                                       |
| ------------------------------ | ---------------------------------------- |
| SET key value                  | 设置指定 key 的值                              |
| GET key                        | 获取指定 key 的值。                             |
| GETRANGE key start end         | 返回 key 中字符串值的子字符                         |
| GETSET key value               | 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。 |
| GETBIT key offset              | 对 key 所储存的字符串值，获取指定偏移量上的位(bit)。          |
| MGET key1 [key2..]             | 获取所有(一个或多个)给定 key 的值。                    |
| SETBIT key offset value        | 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。       |
| SETEX key seconds value        | 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。 |
| SETNX key value                | 只有在 key 不存在时设置 key 的值。                   |
| SETRANGE key offset value      | 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。 |
| STRLEN key                     | 返回 key 所储存的字符串值的长度。                      |
| MSET key value [key value …]   | 同时设置一个或多个 key-value 对。                   |
| MSETNX key value [key value …] | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 |
| PSETEX key milliseconds value  | 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。 |
| INCR key                       | 将 key 中储存的数字值增一。                         |
| INCRBY key increment           | 将 key 所储存的值加上给定的增量值（increment） 。         |
| INCRBYFLOAT key increment      | 将 key 所储存的值加上给定的浮点增量值（increment） 。       |
| DECR key                       | 将 key 中储存的数字值减一。                         |
| DECRBY key decrement           | key 所储存的值减去给定的减量值（decrement） 。           |
| APPEND key value               | 如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。 |

# 列表（List）

## 应用场景

Lists 就是链表，可以通过push和pop操作从列表的头部或者尾部添加或者删除元素，这样List即可以作为栈，也可以作为队列。

使用List结构，可以轻松地实现： 
1、最新消息排行榜。 
2、消息队列，以完成多程序之间的消息交换。可以利用List的PUSH将任务存在List中（生产者），然后工作线程再用POP将任务取出（消费者）。 
3、Redis还提供了操作List中某一段的api，可以直接查询，删除List中某一段的元素。

**应用举例**： 
1.微博关注列表、粉丝列表。 
2.获取最新n条微博（缓存主页或第一个评论页，不用去DB查询）

## 实现方式

Redis list的实现为一个双向链表，Redis内部的很多实现，包括发送缓冲队列等也都是用的这个数据结构。

## 常用接口

| 接口                                    | 介绍                                       |
| ------------------------------------- | ---------------------------------------- |
| BLPOP key1 [key2 ] timeout            | 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| BRPOP key1 [key2 ] timeout            | 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| BRPOPLPUSH source destination timeout | 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| LINDEX key index                      | 通过索引获取列表中的元素                             |
| LINSERT key BEFORE                    | AFTER pivot value                        |
| LLEN key                              | 获取列表长度                                   |
| LPOP key                              | 移出并获取列表的第一个元素                            |
| LPUSH key value1 [value2]             | 将一个或多个值插入到列表头部                           |
| LPUSHX key value                      | 将一个或多个值插入到已存在的列表头部                       |
| LRANGE key start stop                 | 获取列表指定范围内的元素                             |
| LREM key count value                  | 移除列表元素                                   |
| LSET key index value                  | 通过索引设置列表元素的值                             |
| LTRIM key start stop                  | 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| RPOP key                              | 移除并获取列表最后一个元素                            |
| RPOPLPUSH source destination          | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回             |
| RPUSH key value1 [value2]             | 在列表中添加一个或多个值                             |
| RPUSHX key value                      | 为已存在的列表添加值                               |

# 哈希（Hash）

## 应用场景

hash特别适合存储对象。

- 1、相对于将对象的每个字段存成单个string 类型。一个对象存储在hash类型中会占用更少的内存，并且可以更方便的存取整个对象。
- 2、相对于采用String类型的存储对象，需要将对象进行序列化。增加了序列化/反序列化的开销，并且在需要修改其中一项信息时，需要把整个对象取回。而Redis的Hash提供了直接存取这个Map成员的接口。

**应用举例**： 
例如存储、读取、修改用户属性（name，age，pwd等）

## 实现方式

redis的Hash数据类型的value内部是一个HashMap，如果该Map的成员比较少，则会采用一维数组的方式来紧凑存储该Map，省去了大量指针的内存开销，这个参数在redis.conf配置文件中下面2项。

```
Hash-max-zipmap-entries 64
Hash-max-zipmap-value 51212
```

Hash-max-zipmap-entries含义：是当value这个Map内部不超过64个成员时会采用线性紧凑格式存储（默认是64），即value内部有64个以下的成员就是使用线性紧凑存储，超过该值自动转成真正的HashMap。 
Hash-max-zipmap-value含义：是当value这个Map内部的每个成员值长度不超过多少字节就会采用线性紧凑存储来节省空间。

以上两个条件任意一个条件超过设置值都会转成真正的HashMap，也就不会再节省内存了，这个值设置多少需要权衡，HashMap的优势就是查找和操作效率高。

## 常用接口

| 接口                                       | 介绍                                    |
| ---------------------------------------- | ------------------------------------- |
| HDEL key field2 [field2]                 | 删除一个或多个哈希表字段                          |
| HEXISTS key field                        | 查看哈希表 key 中，指定的字段是否存在。                |
| HGET key field                           | 获取存储在哈希表中指定字段的值。                      |
| HGETALL key                              | 获取在哈希表中指定 key 的所有字段和值。                |
| HINCRBY key field increment              | 为哈希表 key 中的指定字段的整数值加上增量 increment 。   |
| HINCRBYFLOAT key field increment         | 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。  |
| HKEYS key                                | 获取所有哈希表中的字段                           |
| HLEN key                                 | 获取哈希表中字段的数量                           |
| HMGET key field1 [field2]                | 获取所有给定字段的值                            |
| HMSET key field1 value1 [field2 value2 ] | 同时将多个 field-value (域-值)对设置到哈希表 key 中。 |
| HSET key field value                     | 将哈希表 key 中的字段 field 的值设为 value 。      |
| HSETNX key field value                   | 只有在字段 field 不存在时，设置哈希表字段的值。           |
| HVALS key                                | 获取哈希表中所有值                             |
| HSCAN key cursor [MATCH pattern] [COUNT count] | 迭代哈希表中的键值对。                           |

注意：Redis提供了接口(HGETALL )可以直接取到全部的属性数据，但是如果内部Map的成员很多，那么涉及到遍历整个内部Map的操作，由于Redis单线程模型的缘故，这个遍历操作可能会比较耗时，而导致其它客户端的请求完全不响应。

# 集合（Set）

## 应用场景

Redis Set对外提供的功能与List类似是一个列表的功能，特殊之处在于Set是自动去重的，Set中的元素是没有顺序的。Set提供了判断某个成员是否在一个Set集合内的接口，这个也是List没有的。

Set集合的概念就是一堆不重复值的组合。比如在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis还为集合提供了求交集、并集、差集等操作，可以非常方便的实现如共同关注、共同喜好、二度好友等功能，对上面的所有集合操作，你还可以使用不同的命令选择将结果返回给客户端还是存集到一个新的集合中。

**应用举例**： 
1.利用交集求共同好友。 
2.利用唯一性，可以统计访问网站的所有独立IP。 
3.好友推荐的时候根据tag求交集，大于某个threshold（临界值的）就可以推荐。

## 实现方式

set 的内部实现是一个value永远为null的HashMap，实际就是通过计算hash的方式来快速去重的，这也是set能提供判断一个成员是否在集合内的原因。

## 常用接口

| 接口                                       | 介绍                                       |
| ---------------------------------------- | ---------------------------------------- |
| SADD key member1 [member2]               | 向集合添加一个或多个成员                             |
| SCARD key                                | 获取集合的成员数                                 |
| SDIFF key1 [key2]                        | 返回给定所有集合的差集                              |
| SDIFFSTORE destination key1 [key2]       | 返回给定所有集合的差集并存储在 destination 中            |
| SINTER key1 [key2]                       | 返回给定所有集合的交集                              |
| SINTERSTORE destination key1 [key2]      | 返回给定所有集合的交集并存储在 destination 中            |
| SISMEMBER key member                     | 判断 member 元素是否是集合 key 的成员                |
| SMEMBERS key                             | 返回集合中的所有成员                               |
| SMOVE source destination member          | 将 member 元素从 source 集合移动到 destination 集合 |
| SPOP key                                 | 移除并返回集合中的一个随机元素                          |
| SRANDMEMBER key [count]                  | 返回集合中一个或多个随机数                            |
| SREM key member1 [member2]               | 移除集合中一个或多个成员                             |
| SUNION key1 [key2]                       | 返回所有给定集合的并集                              |
| SUNIONSTORE destination key1 [key2]      | 所有给定集合的并集存储在 destination 集合中             |
| SSCAN key cursor [MATCH pattern] [COUNT count] | 迭代集合中的元素                                 |

# 有序集合（Sorted Set）

## 使用场景

Redis Sorted Set的使用场景与set类似，区别是Set是无序的，而Sorted Set可以通过用户额外提供一个优先级(score)的参数来为成员排序，并且是插入有序的，即自动排序。当你需要一个有序的并且不重复的集合列表，那么可以选择Sorted Set数据结构，比如微博的timeline（时间线）可以以发表时间作为score来存储，这样获取时就是自动按时间排好序的。

**应用举例**： 
1.timeline（时间线）； 
2.可以用于一个大型在线游戏的积分排行榜，每当玩家的分数发生变化时，可以执行zadd更新玩家分数(score)，此后在通过zrange获取几分top ten的用户信息； 
3.用Sorted Set来做带权重的队列，比如普通消息的score为1，重要消息的score为2，然后工作线程可以选择按score的倒序来获取工作任务。让重要的任务优先执行。

4.秒杀  

## 实现方式

Redis Sorted Set的内部使用HashMap和跳跃表(SkipList)来保证数据的存储和有序，HashMap里放的是成员到score的映射，而跳跃表里存放的是所有的成员，排序依据是HashMap里存的score,使用跳跃表的结构可以获得比较高的查找效率，并且在实现上比较简单。