[TOC]

# redis

Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。

Redis 与其他 key-value 缓存产品相比：支持数据的持久化，多数据结构list，set，zset，hash等的存储，支持数据备份。



## 特点

+ 高性能，可持久化
+ key-value结构，支持多种数据类型
+ 支持失误，数据的原子性（要么不做/全做）



## 应用场景

+ 缓存（读写性能优异）
+ 计数&消息系统（高并发、发布/订阅阻塞队列功能）
+ 分布式会话session&分布式锁（秒杀）



## redis-cli

[命令参考](http://doc.redisfans.com/)



**进入redis-cli：**

```bash
redis-cli
```

备注：如果redis设置鉴权密码，需要使用密码进行鉴权：

```bash
auth 密码
```



**切换数据库：**

以 `0` 作为起始索引值，默认使用 `0` 号数据库。不同的数据库可以进行数据隔离。

```bash
select 数据库索引
```



### 取值/设置

**字符串**

**设置key：**

```bash
set 键名 键值
```



**获取key：**

```bash
get 键名
```



**递增key：**

将 `key` 中储存的数字值增一：

```bash	
incr 键名
```



**递减key：**

将 `key` 中储存的数字值减一。

```bash
decr 键名
```



**查找key：**

查找所有符合给定模式 `pattern` 的 `key` 。

```bash
# 匹配数据库中所有 key 
keys *
# 匹配数据库中key为age
keys age
```

备注：

`KEYS *` 匹配数据库中所有 `key` 。

`KEYS h?llo` 匹配 `hello` ， `hallo` 和 `hxllo` 等。

`KEYS h*llo` 匹配 `hllo` 和 `heeeeello` 等。

`KEYS h[ae]llo` 匹配 `hello` 和 `hallo` ，但不匹配 `hillo` 。



**是否存在key：**

检查给定 `key` 是否存在。

```bash
exists 键名
```



**删除key：**

删除给定的一个或多个 `key` 。

```bash
# 删除单个 key
del 键名
# 同时删除多个 key
del 键名1 键名2 键名3
```



**哈希表**

哈希表适合存储 json 对象类型的数据

**设置key的field：**

将哈希表 `key` 中的域 `field` 的值设为 `value` 。

```bash
hset 键名 域 键值
```



**获取key的field：**

返回哈希表 `key` 中给定域 `field` 的值。

```bash
hget 键名 域
```



**批量设置key的field：**

同时将多个 `field-value` (域-值)对设置到哈希表 `key` 中。

```bash
hmset 键名 域1 键值1 域2 键值2 域3 键值3
```



**批量获取key的field：**

返回哈希表 `key` 中，一个或多个给定域的值。

```bash
hmget 键名 域1 域2 域3
```



**获取所有key的field和value：**

返回哈希表 `key` 中，所有的域和值。

```bash
hgetall 键名
```



**列表**

**获取长度：**

返回列表 `key` 的长度。

```bash
llen 键名
```



**左插入：**

将一个或多个值 `value` 插入到列表 `key` 的表头。如果 `key` 不存在，一个空列表会被创建并执行 lpush 操作。

```bash
lpush 键名 键值1 键值2 键值3
```

备注：lpushx 和 lpush命令相反，当 key 不存在时， lpushx 命令什么也不做。



**右插入：**

将一个或多个值 `value` 插入到列表 `key` 的表尾(最右边)。

```bash
rpush 键名 键值1 键值2 键值3
```

备注：rpushx 和 rpush 命令相反，当 key 不存在时， rpushx 命令什么也不做。



**左移除：**

移除并返回列表 `key` 的头元素。

```bash
lpop 键名 键值
```



**右移除：**

移除并返回列表 `key` 的尾元素。

```bash
rpop 键名 键值
```



**获取指定区间元素：**

返回列表 `key` 中指定区间内的元素，区间以偏移量 `start` 和 `stop` 指定。

```bash
lrange 键名 起始索引 结束索引
```

备注：你也可以使用负数下标，以 `-1` 表示列表的最后一个元素， `-2` 表示列表的倒数第二个元素，以此类推。



**集合**

**设置key加入value：**

将一个或多个 `member` 元素加入到集合 `key` 当中，已经存在于集合的 `member` 元素将被忽略。

```bash
sadd 键名 键值1 键值2 键值3
```



### **发布者/订阅者**

**订阅：**

订阅给定的一个或多个频道的信息。

```bash
subscribe 频道1 频道2 频道3
```



**发布：**

将信息 `message` 发送到指定的频道 `channel` 。

```bash
 publish 频道 消息
```



### 服务器相关

**client list：**

以人类可读的格式，返回所有连接到服务器的客户端信息和统计数据。

```bash
client list
```



**client kill：**

关闭地址为 `ip:port` 的客户端。

```bash
client kill ip:port
```



**flushall：**

清空整个 Redis 服务器的数据(删除所有数据库的所有 key )。

```bash
flushall
```



**flushdb：**

清空当前数据库中的所有 key。

```bash
flushdb
```



**备份：**

SAVE 命令执行一个同步保存操作，将当前 Redis 实例的所有数据快照(snapshot)以 RDB 文件的形式保存到硬盘（文件存放在redis的data目录下的dump.rdb）。

```bash
save
```

重启redis之后，即可恢复备份的数据。



## redis与nodejs集成