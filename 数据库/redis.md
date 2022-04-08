[TOC]

# Redis

## Redis 简介

### Redis 的由来

2008 年，意大利的一家创业公司 Merzia 推出了一款基于 MySQL 的网站实时统计系统 LLOOGG ，然而没过多久该公司的创始人 Salvatore Sanfilippo 便开始对 MySQL 的性能感到失望，于是他决定亲自为 LLOOGG 量身定做一个数据库，并于 2009 年开发完成，这个数据库就是 Redis。

不过 Salvatore Sanfilippo 并不满足只将 Redis 用于 LLOOGG 这一款产品，而是希望让更多的人使用它，于是在同一年 Salvatore Sanfilippo 将 Redis 开源发布，并开始和 Redis 的另一名主要的代码贡献者 Pieter Noordhuis 一起继续着 Redis  的开发，直到今天。

Salvatore Sanfilippo 自己也没有想到，短短的几年时间，Redis 就拥有了庞大的用户群体。国内如BAT、新浪微博、知乎等，国外如 GitHub、Stack Overflow、Flickr、暴雪和 Instagram 等，都是 Redis 的用户。



发展时间线：

- VMware 公司从 2010 年开始赞助 Redis 的开发，Salvatore Sanfilippo 和 Pieter Noordhuis 也分别于同年的 3 月和 5 月加入 VMware，全职开发 Redis
- 而 2013 年 5 月至 2015 年 6 月期间，其开发由 Pivotal 赞助

- 2015 年 6 月，Redis Labs 为进一步的开发进行赞助一直到现在



Redis 代码托管在 GitHub 上（https://github.com/redis/redis），开发十分活跃。



### 什么是 Redis   

以下是官方文档的解释。

Redis 是一个使用 ANSI C 编写的开源、支持网络、基于内存、可选持久性的键值对存储数据库。

Redis 不是简单的 Key-Value 数据库，它还支持数据结构，例如

- 字符串
- 哈希

- 列表
- 集合

- 带范围查询的排序集合
- 位图

- 超日志
- 带有半径查询和流的地理空间索引

Redis 具有内置的复制功能，解析执行 Lua 脚本，LRU 缓存控制，事务和不同级别的磁盘持久性，并通过 Redis Sentinel 和 Redis Cluster 自动分区提供高可用性。



#### Redis 的存储结构

在大多数编程语言中都有一种数据结构：字典，例如代码 `dict["key"] = "value"` 中：

- `dict` 是一个字典结构变量
- `key` 是键名

- `value` 是键值

在字典中我们可以获取或设置键名对应的键值，也可以删除一个键。

Redis 是 REmote DIctionary Server（远程字典服务器）的缩写，它以字典结构存储数据，并允许其他应用通过 TCP 协议读写字典中的内容。

Redis 字典中的键值除了可以是字符串，还可以是其它数据类型。其中比较常见的有：

| 类型   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| String | 字符串                                                       |
| Hash   | 散列，是由与值相关联的字段组成的内容。字段和值都是字符串。这与 Ruby 或 Python 哈希非常相似。类似于 JavaScript 中的对象结构。 |
| List   | 列表，根据插入顺序排序的字符串元素的集合。它们基本上是链表。 |
| Set    | 未排序的字符串元素集合，集合中的数据是不重复的               |
| ZSet   | 与Sets类似，但每个字符串元素都与一个称为分数的浮点值相关联。元素总是按它们的分数排序，因此与 Sets 不同，可以检索一系列元素（例如，您可能会问：给我前10名或后10名） |



#### 内存存储与持久化

Redis 数据库中所有数据都存储在内存中。相对于磁盘，内存的数据读/写速度要快得多，所以我们通常用 Redis 做缓存数据库，在一台普通电脑上，Redis 可以在一秒内读写超过 10 万个键值。

Redis 官网的性能测试显示，在 Linux 2.6、Xeon X3320 2.5 GHz 服务器上，50 个并发的情况下请求 100000 次，SET 操作可达 110000 次/s，GET 操作可达 81000 次/s

将数据存储在内存中也有问题，比如程序退出后内存中的数据会丢失。不过 Redis 提供了对持久化的支持，即可以将内存中的数据异步写入到硬盘中，同时不影响继续提供服务。



#### 功能丰富

Redis 虽然是作为数据库开发的，但是由于提供了丰富的功能，越来越多人将其用作缓存、队列系统等。

（1）作为缓存系统

Redis 可以为每个键设置生存时间，生存时间到期后会自动被删除。这一功能配合出色的性能让 Redis 可以作为缓存来使用。作为缓存系统，Redis 还可以限定数据占用的最大空间，在数据达到空间限制后可以按照一定的规则自动淘汰不需要的键。



（2）作为队列系统

除此之外，Redis 的列表类型键可以用来实现队列，并且支持阻塞式读取，可以很容易的实现一个高性能的优先级队列。



（3）“发布/订阅”功能

同时在更高层面上，Redis 还支持“发布/订阅”的消息模式，可以基于此构建聊天室等系统。



#### 简单稳定

即使功能再丰富，如果使用起来太复杂也很难吸引人。Redis 直观的存储结构使得通过程序与 Redis 交互十分简单。

在 Redis 中使用命令来读写数据，命令语句之于 Redis 就相当于 SQL 语言之于关系数据库。例如在关系数据库中要获取 posts 表内 id 为 1 的记录的 title 字段可以使用如下 SQL 语句实现：

```sql
SELECT title FROM posts WHERE id=1 LIMIT 1
```

相对应的，在 Redis 中要读取键名为 post: 1 的散列类型键的 title 字段的值，可以使用如下语句实现：

```shell
HGET post:1 title
```

其中，HGET 就是一个命令，`post:1` 是键名，`title` 是要读取的数据字段。



Redis 提供了 250 多个命令，听起来很多，但是常用的也就几十个，并且每个命令都很容易记忆。

![img](Redis.assets/1605584047906-5cf4bb91-6144-41e8-97b6-2354e833c1f0-2748200.png)

Redis 命令列表：https://redis.io/commands



Redis 提供了几十种不同编程语言的客户端（https://redis.io/clients），这些库都很好的封装了 Redis 的命令，使得在程序中与 Redis 进行交互变得更容易。



### Redis 应用场景

Redis是一个 Key-Value 存储系统，大部分情况下是因为其高性能的特性，被当做缓存使用，这里介绍下Redis经常遇到的使用场景。

一个产品的使用场景肯定是需要根据产品的特性，先列举一下 Redis 的特点：

- 读写性能优异
- 持久化

- 数据类型丰富
- 单线程

- 数据自动过期
- 发布订阅

- 分布式



这里我们通过几个场景，不同维度说下 Redis 的应用。

**（1）缓存系统**

缓存现在几乎是所有中大型网站都在用的必杀技，合理的利用缓存不仅能够提升网站访问速度，还能大大降低数据库的压力。Redis 提供了键过期功能，也提供了灵活的键淘汰策略，所以，现在 Redis 用在缓存的场合非常多。



**（2）排行榜**

很多网站都有排行榜应用的，如京东的月度销量榜单、商品按时间的上新排行榜等。Redis提供的有序集合数据类构能实现各种复杂的排行榜应用。



**（3）计数器**

什么是计数器，如电商网站商品的浏览量、视频网站视频的播放数等。为了保证数据实时效，每次浏览都得给+1，并发量高时如果每次都请求数据库操作无疑是种挑战和压力。Redis提供的incr命令来实现计数器功能，内存操作，性能非常好，非常适用于这些计数场景。



**（4）分布式会话**

集群模式下，在应用不多的情况下一般使用容器自带的session复制功能就能满足，当应用增多相对复杂的系统中，一般都会搭建以Redis等内存数据库为中心的session服务，session不再由容器管理，而是由session服务及内存数据库管理。



**（5）分布式锁**

在很多互联网公司中都使用了分布式技术，分布式技术带来的技术挑战是对同一个资源的并发访问，如全局ID、减库存、秒杀等场景，并发量不大的场景可以使用数据库的悲观锁、乐观锁来实现，但在并发量高的场合中，利用数据库锁来控制资源的并发访问是不太理想的，大大影响了数据库的性能。可以利用Redis的setnx功能来编写分布式的锁，如果设置返回1说明获取锁成功，否则获取锁失败，实际应用中要考虑的细节要更多。



**（6）社交网络**

点赞、踩、关注/被关注、共同好友等是社交网站的基本功能，社交网站的访问量通常来说比较大，而且传统的关系数据库类型不适合存储这种类型的数据，Redis提供的哈希、集合等数据结构能很方便的的实现这些功能。



**（7）最新列表**

Redis列表结构，LPUSH可以在列表头部插入一个内容ID作为关键字，LTRIM可用来限制列表的数量，这样列表永远为N个ID，无需查询最新的列表，直接根据ID去到对应的内容页即可。



**（8）消息系统**

消息队列是大型网站必用中间件，如 ActiveMQ、RabbitMQ、Kafka 等流行的消息队列中间件，主要用于业务解耦、流量削峰及异步处理实时性低的业务。Redis 提供了发布/订阅及阻塞队列功能，能实现一个简单的消息队列系统。另外，这个不能和专业的消息中间件相比。



**（9）示例：秒杀和 Redis 的结合**

秒杀是现在互联网系统中常见的营销模式，作为开发者，其实最不愿意这样的活动，因为非技术人员无法理解到其中的技术难度，导致在资源协调上总是有些偏差。秒杀其实经常会出现的问题包括：

- 并发太高导致程序阻塞。
- 库存无法有效控制，出现超卖的情况。



其实解决这些问题基本就两个方案：

- 数据尽量缓存,阻断用户和数据库的直接交互。
- 通过锁来控制避免超卖现象。



现在说明一下，如果现在做一个秒杀，那么，Redis 应该如何结合进行使用?

- 提前预热数据，放入Redis
- 商品列表放入Redis List

- 商品的详情数据 Redis Hash 保存，设置过期时间
- 商品的库存数据Redis Sorted Set 保存

- 用户的地址信息 Redis Set 保存
- 订单产生扣库存通过 Redis 制造分布式锁，库存同步扣除

- 订单产生后发货的数据，产生 Redis List，通过消息队列处理
- 秒杀结束后，再把 Redis 数据和数据库进行同步

以上是一个简略的秒杀系统和 Redis 结合的方案，当然实际可能还会引入 HTTP 缓存，或者将消息对接用 MQ 代替等方案，也会出现业务遗漏的情况，这个只是希望能抛砖引玉。



### 相关资源

- 官网：https://redis.io/
- GitHub 仓库：https://github.com/redis/redis

- 交互式学习 Redis：[https://try.redis.io/](https://try.redis.io/?_ga=2.261967822.2140461275.1605261471-2058170936.1604993353)
- Redis 中文网（非官方）：http://www.redis.cn/

- Redis 命令参考：http://doc.redisfans.com/





## Redis 安装

### 关于 Redis 的版本

Redis 借鉴了 Linux 操作系统对于版本号的命名规则：

- 版本号第二位如果是奇数，则为非稳定版本（例如2.7、2.9、3.1）
- 如果是偶数，则为稳定版本（例如2.6、2.8、3.0、3.2）



当前奇数版本就是下一个稳定版本的开发版本，例如 2.9 版本是 3.0 版本的开发版本。所以我们在生产环境通常选取偶数版本的Redis，如果对于某些新的特性想提前了解和使用，可以选择最新的奇数版本。



### 获取 Redis 的方式

获取 Redis 的方式有很多种：

- 安装到自己电脑上
- 安装到虚拟机上

- 安装到远程服务器上
- 可以从 Docker Hub 获取 Redis 的 Docker 镜像

- ...



### 在 macOS 中安装 Redis

在 macOS 中有两种方式：

- 方式一：编译安装
- 方式二（推荐）：使用 Homebrew 安装



macOS 系统下的软件包管理工具 Homebrew 提供了较新版本的 Redis 包，所以我们可以直接使用它们来安装 Redis，省去了在 Linux 上需要手动编译的麻烦。

1、安装 [Homebrew](https://brew.sh/)

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```



2、通过 Homebrew 安装 Redis

```shell
brew install redis
```



### 在 Windows 中安装 Redis

Redis 官方不支持 Windows。2011 年微软向 Redis 提交了一个补丁，以使 Redis 可以在 Windows 下编译运行。但被作者拒绝了，原因是在服务器领域上 Linux 已经得到了广泛的使用，让 Redis 能在 Windows 下运行相比而言显得不那么重要。并且 Redis 使用了如写时复制等很多操作系统相关的特性，兼容 Windows 会耗费天大的精力而影响 Redis 的开发。

尽管如此微软还是发布了一个可以在 [Windows 下的 Redis 版本](https://github.com/microsoftarchive/redis)，但是这个项目已经不再维护。

如果你实在想要在 Windows 上学习使用 Redis 的话可以尝试一下 [Memurai](https://www.memurai.com/)，它是一个 Redis for Windows 的替代品，它的核心基于 Redis 源代码并且完全兼容 Redis，但是该项目并未得到微软官方的认可，有兴趣的话可以尝试一下。



### 在 Linux 中安装 Redis

```shell
# 下载 Redis 源码
wget https://download.redis.io/releases/redis-6.0.9.tar.gz

# 解压 Redis 压缩包
tar xzf redis-6.0.9.tar.gz

# 进入 Redis 源码目录
cd redis-6.0.9

# 编译安装
make
```

现在已编译的二进制文件位于 `src` 目录中。使用以下命令运行 Redis：

```shell
$ ./src/redis-server
```



要将 Redis 二进制文件安装到 `/usr/local/bin` 中，只需使用：

```shell
make install
```



### 运行 Redis

编译后在 Redis 源代码目录的 `src` 文件夹中会有以下几个可执行文件：

| **可执行文件**   | **说明**           |
| ---------------- | ------------------ |
| redis-server     | Redis 服务器       |
| redis-cli        | Redis 命令行客户端 |
| redis-benchmark  | Redis 性能测试工具 |
| redis-check-aof  | AOF 文件修复工具   |
| redis-check-dump | RDB 文件检查工具   |
| redis-sentinel   | 哨兵模式工具       |

注意：通过编译源码安装的话，也会产生一个 `redis.conf` 的配置文件。



我们最常用是 `redis-server` 和 `redis-cli`。



最简单的，直接运行 `redis-server` 即可启动 Redis：

```shell
redis-server
```

Redis 默认使用 `6379` 端口，我们也可以通过 `--port` 参数指定启动端口：

```shell
redis-server --port 1234
```



如果需要在后端运行 Redis：

```shell
redis-server --daemonize yes
```

查看 Redis 运行状态：

```shell
# 查看 Redis 后端运行进程
ps -ef | grep -i redis
```



### 停止 Redis

考虑到 Redis 有可能正在将内存中的数据同步到硬盘中，强行终止 Redis 进程可能会导致数据丢失。

所有正确停止 Redis 的方式应该是向 Redis 发送 SHUTDOWN  命令：

```shell
redis-cli shutdown
```

当 Redis 手动 Shutdown 命令后，会先断开所有客户端连接，然后根据配置执行持久化，最后完成退出。



Redis 可以妥善处理 SIGTERM 信号，所有使用 kill Redis 进程的 PID 也可以正常结束 Redis，效果与发送 SHUTDOWN 命令一样。

```shell
# 通过进程号停止 Redis
kill -9 4684
```



### 连接 Redis

redis-cli 是 Redis 自带的基于命令行的 Redis 客户端，也是我们学习和测试 Redis 的重要工具。

运行 `redis-cli` 即可连接数据库：

```shell
redis-cli
```

也可以指定服务器地址和端口连接：

```shell
redis-cli -h 127.0.0.1 -p 1234
```

不出差错的话，此时已经连接上 Redis 数据库，我们通过 Redis 提供的 `PING` 命令来测试与 Redis 是否连接正常：

```shell
127.0.0.1:6379> PING
PONG
127.0.0.1:6379>
```

Redis 返回 `PONG`，证明连接正常。



如果想要断开连接：

- 命令：`quit`
- 快捷键：`Ctrl + C`



## Redis 配置

我们在之前介绍过可以通过 redis-server 的启动参数 port 设置了 Redis 服务的端口号，除此之外 Redis 还支持其他配置选项，如是否开启持久化、日志级别等。



### 通过命令行传递参数

最简单的方式就是在启动 redis-server 的时候直接传递命令参数。

```shell
redis-server --port 6380 --host 127.0.0.1
```



### 配置文件

由于可以配置的选项较多，通过启动参数设置这些选项并不方便，所以 Redis 支持通过配置文件来设置这些选项。

Redis 提供了一个配置文件的模板 redis.conf，位于源代码目录的根目录中。



我们建议把该文件放到 `/etc/redis` 目录中（该目录需要手动创建），以端口号命令，例如 `6379.conf`。



启用配置文件的方法是在启动时将配置文件的路径作为启动参数传递给 redis-server：

```shell
redis-server 配置文件路径
```



通过启动参数传递同名的配置选项会覆盖配置文件中的相应的参数，就像这样：

```shell
redis-server 配置文件路径 --port 3000
```



### 在服务器运行时更改 Redis 配置

还可以在 Redis 运行时通过 CONFIG SET 命令在不重新启动 Redis 的清空下动态修改部分 Redis 配置。就像这样：

```shell
CONFIG SET logLevel warning
```



同样在运行的时候也可以使用 CONFIG GET 命令获得 Redis 当前的配置情况：

```shell
CONFIG GET logLevel
```





## Redis 中的多数据库

一个 Redis 实例提供了多个用来存储数据的字典，客户端可以指定将数据存储在哪个字典中。这与我们熟知的在一个关系数据库中可以创建多个数据库类似，所有可以将其中的每个字典都理解成一个独立的数据库。



Redis 默认支持 16 个数据库，分别编号为 0、1、2、...14、15

- Redis 不支持自定义数据库名字
- 因为每个数据库都以编号命名，所有开发者必须要明确哪个数据库存放了哪些数据

- 可以通过配置参数 `databases` 修改支持的数据库个数



每个数据库都是独立的，也就是说你在 0 号数据库中插入的数据在 1 号数据库是访问不到的。

客户端与 Redis 建立连接后自动选择 0 号数据库，我们可以使用 `SELECT` 命令来更换数据库。

```shell
127.0.0.1:6379> SET a 1
OK
127.0.0.1:6379> KEYS *
1) "a"
127.0.0.1:6379> SELECT 16
(error) ERR DB index is out of range
127.0.0.1:6379> SELECT 15
OK
127.0.0.1:6379[15]> SET b 2
OK
127.0.0.1:6379[15]> KEYS *
1) "b"
127.0.0.1:6379[15]> SELECT 0
OK
127.0.0.1:6379> KEYS *
1) "a"
127.0.0.1:6379>

# 将指定 key 移动到指定数据库
move key db
```

当选择的数据库编号超过最大数据库编号时，默认编号的数据库

**Redis 不支持为每个数据库设置不同的访问密码，所有一个客户端要么可以访问全部数据库，要么一个数据库也没有权限访问。**

最重要的一点是多个数据库之间并不是完全隔离的，比如 `FLUSHALL` 命令可以清空一个 Redis 实例中所有数据库中的数据。

综上所述，这些数据库更像是一个命名空间，而**不适宜存储不同应用程序的数据**，比如不适宜使用 0 号数据库存储 A 应用数据而使用 1 号数据库存储 B 应用数据，这是非常不推荐的做法！！！

**不同的应用应该使用不同的 Redis 实例存储数据。**由于 Redis 非常轻量级，一个空的 Redis 占用的内存只有 1 MB 作用，所以不用担心多个 Redis 实例会额外占用很多内存的问题。



## Redis 常用数据类型及操作命令（CRUD）

Redis 不是简单的键值存储，它实际上是一个数据结构服务器，支持不同类型的值。这意味着在传统键值存储中，您将字符串键与字符串值相关联，而在 Redis 中，该值不仅限于简单的字符串，还可以容纳更复杂的数据结构。以下是 Redis 中支持的所有数据结构的列表：

| 类型                     | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| String                   | 字符串                                                       |
| Hash                     | 散列，是由与值相关联的字段组成的内容。字段和值都是字符串。这与 Ruby 或 Python 哈希非常相似。类似于 JavaScript 中的对象结构。 |
| List                     | 列表，根据插入顺序排序的字符串元素的集合。它们基本上是链表。 |
| Set                      | 未排序的字符串元素集合，集合中的数据是不重复的。             |
| ZSet                     | 与 Sets 类似，但每个字符串元素都与一个称为分数的浮点值相关联。元素总是按它们的分数排序，因此与 Sets 不同，可以检索一系列元素（例如，您可能会问：给我前10名或后10名）。 |
| Bit arrays（或 bitmaps） | 可以使用特殊命令像位数组一样处理字符串值：您可以设置和清除单个位，计数所有设置为1的位，找到第一个设置或未设置的位，依此类推。 |
| HyperLogLogs             | 这是一个概率数据结构，用于估计集合的基数。                   |
| Streams                  | 提供抽象日志数据类型的类似地图项的仅追加集合。               |



### Redis 中的键

Redis 密钥是二进制安全的，这意味着您可以使用任何二进制序列作为 key，从 `"foo"` 之类的字符串到 JPEG 文件的内容。空字符串也是有效的键。

有关键的其他一些规则：

- 太长不好，占用内存空间
- 太短也不好，没有可读性

- 尝试坚持使用固定规则，例如：

- - `object-type:id`
  - `user:1000`

- - 点或破折号通常用于多字字段，例如：`comment:1234:reply.to` 或 `comment:1234:reply-to` 中。

- 允许的最大大小为 512 MB



总结一下：

- 不要太长，浪费空间
- 不要过短，不利于阅读

- 统一的命令规范



### 字符串（String）

字符串类型是 Redis 中最基本的数据类型，也是其它数据类型的基础。

- 它能存储任何形式的字符串，包括二进制数据。
- 你可以用它存储用户的邮箱、JSON 化的对象，甚至是一张图片

- value 最多可以容纳数据大小为 512 MB

![img](Redis.assets/1605629735294-ee2beae8-8fb3-47bb-955e-05cb39e34c0e-2748966.png)



字符串类型是其他常见 4 种数据类型的基础，其他数据类型和字符串类型的差别从某种角度来说只是组织字符的形式不同。

例如，列表类型是以列表的形式组织字符串，而集合类型是以集合的形式组织字符串。学习完后面的数据类型之后相信你会有更深的理解。



#### 添加

```shell
# 设置指定 key 的值
SET key value

# 将给定 key 的值设为 value ，并返回 key 的旧值(old value)
GETSET key value

# 只有在 key 不存在时设置 key 的值
SETNX key value

# 同时设置一个或多个 key-value 对
MSET key value [key value ...]

# 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在
MSETNX key value [key value ...]

# 如果 key 已经存在并且是一个字符串， APPEND 命令将指定的 value 追加到该 key 原来值（value）的末尾。
APPEND key value
```

注意：在 Redis 中命令不区分大小写。也就是说 `SET foo bar` 和 `set foo bar` 是一样的，但是我们约定使用大写表示它是一个 Redis 命令。



#### 查询

```shell
# 获取指定 key 的值
GET key

# 返回 key 中字符串值的子字符
GETRANGE key start end

# 获取所有(一个或多个)给定 key 的值
MGET key [key ...]

# 返回 key 所储存的字符串值的长度。
STRLEN key

# 通用命令：查询集合中是否有指定的 key
EXISTS key [key ...]

# 通用命令，查询 key 的类型
TYPE key
```



#### 修改

```shell
# 设置指定 key 的值
SET key value

# 将给定 key 的值设为 value ，并返回 key 的旧值(old value)
GETSET key value

# 如果 key 已经存在并且是一个字符串， APPEND 命令将指定的 value 追加到该 key 原来值（value）的末尾。
APPEND key value
```



#### 删除

```shell
# 通用命令：删除1个或多个指定的 key
DEL key [key ...]
```



### 数字值

数字值在 Redis 中以字符串保存。

```shell
# 将 key 中储存的数字值增一
INCR key

# 将 key 所储存的值加上给定的增量值（increment） 
INCRBY key increment

# 将 key 中储存的数字值减一
DECR key

# key 所储存的值减去给定的减量值（decrement）
DECRBY key decrement
```



### 哈希（Hash）

哈希（也叫散列）类型也是一种字典结构，其存储了字段和字段值的映射，但字符值只能是字符串，不能其它数据类型，换句话说，散列类型不能嵌套其它数据类型。一个哈希类型可以包含至少 232 - 1 个字段。

提示：除了散列类型，Redis 的其它数据类型同样不支持数据类型嵌套。

![img](Redis.assets/1605605955508-673cd046-6020-4c17-8588-3b32213b6b85.png)



#### 添加

```shell
# 将哈希表 key 中的字段 field 的值设为 value
HSET key field value [field value ...]

# 同时将多个 field-value (域-值)对设置到哈希表 key 中
HMSET key field value [field value ...]

# 只有在字段 field 不存在时，设置哈希表字段的值
HSETNX key field value
```



#### 查询

```shell
# 获取所有哈希表中的字段
HKEYS key

# 获取哈希表中字段的数量
HLEN key

# 获取所有给定字段的值
HMGET key field1 [field2]

# 获取存储在哈希表中指定字段的值
HGET key field

# 获取在哈希表中指定 key 的所有字段和值
HGETALL key

# 查看哈希表 key 中，指定的字段是否存在
HEXISTS key field

# 获取哈希表中所有值
HVALS key

# 迭代哈希表中的键值对
HSCAN key cursor [MATCH pattern] [COUNT count]
```



#### 修改

```shell
# 将哈希表 key 中的字段 field 的值设为 value
HSET key field value [field value ...]

# 为哈希表 key 中的指定字段的整数值加上增量 increment
HINCRBY key field increment
```



#### 删除

```shell
# 删除一个或多个哈希表字段
HDEL key field1 [field2]

# 删除整个数据字段
DEL key [key ...]
```



### 列表（List）

列表类型类似于编程语言中的数组，可以存储一个有序的字符串列表，常用的操作就是向列表两端添加元素，或者获得列表的某一个片段。

![img](Redis.assets/1605629929475-1fd5bb14-b6fb-4a76-931d-9aea95b46205-2749084.png)

列表类型内部使用双向链表实现的，所有向列表两端添加元素的时间复杂度为O(1)，获取越接近两端的元素速度就越快。这意味着即时是一个有几千万个元素的列表，获取头部或尾部的10条记录也是极快的（和从只有20个元素的列表中获取头部或尾部的10条记录的速度是一样的）。

不过使用链表的代价是通过索引访问元素比较慢。设想在 iPhone 发售当前有 1000 个人在商店排队购买，这时商家为了感谢大家的支持，决定奖励第486位的顾客异步免费的 iPhone。为了找到这第 486 位顾客，工作人员不得不从队首一个一个地数到 486 个人。但同时，无论队伍有多长，新来的人想加入队伍的话直接排到队尾就好了，和队伍里有多少人没有任何关系。这种情景与列表类型的特性很相似。

这种特性使列表类型能非常快速地完成关系数据库难以应付的场景：例如社交网站的新鲜事，我们关心的只是最新内容，使用列表类型存储，即使新鲜事的总数达到几千万个，获取其中最新的100条数据也是极快的。同样因为在两端插入记录的时间复杂度是O(1)，列表类型也适合用来记录日志，可以保证加入新日志的速度不会受到已有日志数量额影响。

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。



#### 添加

```shell
# 将一个或多个值插入到列表头部
LPUSH key element [element ...]

# 在列表的元素前或者后插入元素
LINSERT key BEFORE|AFTER pivot value

# 将一个值插入到已存在的列表头部
LPUSHX key value

# 通过索引设置列表元素的值
LSET key index value

# 在列表中添加一个或多个值
RPUSH key value1 [value2]

# 为已存在的列表添加值
RPUSHX key value
```



#### 查询

```shell
# 通过索引获取列表中的元素
LINDEX key index

# 获取列表长度
LLEN key

# 获取列表指定范围内的元素
LRANGE key start stop
```



#### 删除

```shell
# 移出并获取列表的第一个元素
LPOP key

# 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
BLPOP key1 [key2 ] timeout

# 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
BRPOP key1 [key2 ] timeout

# 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
BRPOPLPUSH source destination timeout

# 移除列表元素
# 如果 count > 0，则从头向尾遍历删除元素
# 如果 count < 0，则从后面向前面删除元素
# 如果 count = 0，则删除所有匹配的元素
LREM key count value

# 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除
LTRIM key start stop

# 移除列表的最后一个元素，返回值为移除的元素
RPOP key

# 移除列表的最后一个元素，并将该元素添加到另一个列表并返回
RPOPLPUSH source destination
```





### 集合（Set）

集合类型和数学中的集合概念相似，集合中的元素是唯一的、无序的，简单理解集合就是没有顺序且不重复的列表。

一个集合类型可以存储至多 232 - 1 个字符串。

集合类型和列表类型有相似之处，它们的主要区别是：

- 列表是有序的，集合是无序的
- 列表数据可以重复，集合中没有重复数据

集合类型的常用操作是向集合中加入或删除元素、判断某个元素是否存在等。由于集合类型在 Redis 内部是使用值为空的散列表实现的，所以这些操作的时间复杂度都是O(1)。

最方便的是多个集合之间还可以进行并集、交集和差集运算。



#### 添加

```shell
# 向集合添加一个或多个成员
SADD key member1 [member2]
```



#### 查询

```shell
# 返回集合中的所有成员
SMEMBERS key

# 获取集合的成员数
SCARD key

# 判断 member 元素是否是集合 key 的成员
SISMEMBER key member

# 返回集合中一个或多个随机数
SRANDMEMBER key [count]
```



#### 删除

```shell
# 移除集合中一个或多个成员
SREM key member1 [member2]

# 移除并返回集合中的一个随机元素
SPOP key

# 将 member 元素从 source 集合移动到 destination 集合
SMOVE source destination member
```



#### 集合间聚合运算

多个集合之间还可以进行**并集**、**交集**和**差集**运算。

```shell
# 返回第一个集合与其他集合之间的差异。
SDIFF key1 [key2]

# 返回给定所有集合的交集
SINTER key1 [key2]

# 返回所有给定集合的并集
SUNION key1 [key2]

# 返回给定所有集合的差集并存储在 destination 中
SDIFFSTORE destination key1 [key2]

# 返回给定所有集合的交集并存储在 destination 中
SINTERSTORE destination key1 [key2]

# 所有给定集合的并集存储在 destination 集合中
SUNIONSTORE destination key1 [key2]
```



#### 使用场景

- 跟踪一些唯一性数据

- - 比如访问网站的唯一 IP 地址信息，每次访问网站的时候记录用户 IP 地址，SET 自动保证数据的唯一不重复

- 充分利用 SET 聚合操作方便高效的特性，用于维护数据对象之间的关联关系

- - 比如所有购买A商品的客户 ID 存储到指定的 SET 中，所有购买B商品的客户 ID 存储到指定的 SET 中，如果我们想要获取有哪个客户同时购买了这两个商品，我们只需要使用交集操作就可以轻松的查出来







### 有序集合（Sorted Set）

有序集合是一种类似于集合和哈希之间的混合数据类型。

- 与集合一样，排序集合由唯一的非重复字符串元素组成
- 有序集合中的元素不排序，但有序集合中的每个元素都关联了一个分数（这就是为什么类型也类似于哈希，因为每个元素都映射到一个值）

- 虽然集合中每个元素都是不同的，但是它们的分数确可以相同

![img](Redis.assets/1605670677158-17074bb8-4ee0-478b-a319-d1b7a81aeef2-2749186.png)

每个元素都会关联一个 double 类型的分数。Redis 正是通过分数来为集合中的成员进行从小到大的排序。

有序集合类型在某些方面和列表类型有些相似。

相同点：

- 两者都是有序的
- 两者都可以获得某一范围的元素



不同点：

- 列表类型通过链表实现的，获取靠近两端的数据速度极快，而当元素增多后，访问中间数据的速度会较慢，所以它更适合实现如“新鲜事”或“日志”这样很少访问中间元素的应用
- 有序集合类似是使用哈希表实现的，所以即使读取位于中间部分的数据速度也很快

- 列表中不能简单的调整某个元素的位置，但是有序集合可以（通过更改元素的分数）
- 有序集合要比列表类型更耗费内存



有序集合的典型应用场景：

（1）排行榜

例如一个大型在线游戏的积分排行榜，每当玩家的分数发生变化时，可以执行 `ZADD` 命令更新玩家的分数，此后再通过 `ZRANGE` 命令获取积分 TOPTEN 的用户信息。当然我们也可以利用 `ZRANK` 命令通过 username 来获取玩家的排行信息。最后我们将组合使用 `ZRANGE` 和 `ZRANK` 命令快速的获取和某个玩家积分相近的其他用户的信息。



（2）微博热搜

假设我们现在要获取热门的帖子或搜索，比如我们常用的微博热搜。

首先，我们需要一个衡量的标准，定量的量度热搜的热门程度。假设我们有一个字段叫回复量，回复量越高就越热门。

如果我们用关系型数据库来获取的话，用 SQL 语句实现很简单：

```sql
SELECT * FROM message ORDER BY backsum LIMIT 10
```

但是当数据量很大的时候，效率很低，同时如果建立索引又要消耗大量的资源，同时增加负载。

使用 Redis 的时候，我们不需要存储多余的信息，只需要存储帖子 id 和回复量两个信息就可以了。



#### 添加

```shell
# 向有序集合添加一个或多个成员，或者更新已存在成员的分数
ZADD key score member [score member ...]
```



#### 查询

```shell
# 通过索引区间返回有序集合指定区间内的成员，分数从低到高排序
ZRANGE key start stop [WITHSCORES]

# 通过索引区间返回有序集合指定区间内的成员，分数从高到低排序
ZREVRANGE key start stop [WITHSCORES]

# 返回有序集中指定分数区间内的成员，分数从低到高排序
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]

# 返回有序集中指定分数区间内的成员，分数从高到低排序
ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]

# 返回有序集合中指定成员的排名，有序集成员按分数值（从小到大）排序
ZRANK key member

# 返回有序集合中指定成员的排名，有序集成员按分数值（从大到小）排序
ZREVRANK key member

# 获取有序集合的成员数
ZCARD key

# 返回有序集中，成员的分数值
ZSCORE key member

# 计算在有序集合中指定区间分数的成员数
ZCOUNT key min max
```



#### 修改

```shell
# 向有序集合添加一个或多个成员，或者更新已存在成员的分数
ZADD key score member [score member ...]

# 有序集合中对指定成员的分数加上增量 increment
ZINCRBY key increment member
```



#### 删除

```shell
# 移除有序集合中的一个或多个成员
ZREM key member [member ...]

# 移除有序集合中给定的排名区间的所有成员
ZREMRANGEBYRANK key start stop

# 移除有序集合中给定的分数区间的所有成员
ZREMRANGEBYSCORE key min max
```



#### 有序集合间聚合运算

```shell
# 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 destination 中
ZINTERSTORE destination numkeys key [key ...]

# 计算给定的一个或多个有序集的并集，并存储在新的 key 中
ZUNIONSTORE destination numkeys key [key ...]
```



### 通用命令

```shell
# 返回所有 key
KEYS *

# 返回所有以 my 开头的 key
KEYS my*

# 获取 key 的类型
TYPE key

# 查询某个 key 是否存在
EXISTS key [key ...]

# 将 key 改名为 newkey
RENAME key newkey

# 删除指定 key
DEL key [key ...]

# 从当前数据库中随机返回(不删除)一个 key
RANDOMKEY

# 对 key 进行重命名
RENAME key newkey

# 清空当前数据库所有内容
FLUSHDB

# 清空所有数据库内容
FLUSHALL

# 将当前数据库的 key 移动到给定的数据库 db 当中
MOVE key db
```



## Redis 过期时间

在实际开发中经常会遇到一些有时效的数据，比如限时优惠活动、缓存或验证码等，过了一定时间就需要删除这些数据。在关系数据库中一般需要额外的一个字段记录到期时间，然后定期检测删除过期数据。而在 Redis 中可以设置一个键的过期时间，到时间后 Redis 会自动删除它。



### 设置键的过期时间

```shell
# 为给定 key 设置生存时间，当 key 过期时(生存时间为 0 )，它会被自动删除。
EXPIRE key seconds

# 和 EXPIRE 一样，但是它以毫秒为单位
PEXPIRE key milliseconds

# EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置生存时间。
# 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。
EXPIREAT key timestamp

# 这个命令和 EXPIREAT 命令类似，但它以毫秒为单位设置 key 的过期 unix 时间戳，而不是像 EXPIREAT 那样，以秒为单位。
PEXPIREAT key milliseconds-timestamp
```

上面这4个命令只是单位和表现形式上的不同，但实际上 EXPIRE、PEXPIRE 以及 EXPIREAT 命令的执行最后都会使用 PEXPIREAT 来实行。



比如使用 `EXPIRE` 来设置 KEY 的生存时间为 N 秒，那么后台是如何运行的呢：

- 它会调用 `PEXPIRE` 命令把 N 秒转换为M毫秒
- 然后获取当前的 UNIX 时间单位也是毫秒

- 把当前 UNIX 时间加上 M 毫秒传递给 `PEXPREAT`



另外给键设置了过期时间，这个时间保存在一个字典里，也是键值结构，键是一个指针，指向真实的键，而值这是一个长整型的 UNIX 时间。



### 获取键的过期时间

```shell
# 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。
TTL key

# 类似于 TTL，但它以毫秒为单位返回 key 的剩余生存时间。
PTTL key
```



过期时间返回值说明：

| **值** | **说明**                       |
| ------ | ------------------------------ |
| -2     | 过期且已删除                   |
| -1     | 没有过期时间设置，即永不过期   |
| >0     | 表示距离过期还有多少秒或者毫秒 |



### 清除键的过期时间

```shell
# 移除给定 key 的生存时间，将这个 key 从『易失的』(带生存时间 key )转换成『持久的』(一个不带生存时间、永不过期的 key )。
PERSIST key
```

注意：

- 使用 SET 或 GETSET 命令为键赋值也会同时清除键的过期时间。
- 其它只对键值进行操作的命令（如 INCR、LPUSH、HSET、ZREM）不会影响键的过期时间。



## Redis 事务

### 事务的基础概念

关于事务最常见的例子就是银行转账，A 账户给 B 账户转账一个亿 (T1)，买一块地盖房子。在这种交易的过程中，有几个问题值得思考：

- 如何**同时保证**上述交易中，A账户总金额减少一个亿，B账户总金额增加一个亿？ A
- A账户如果同时在和C账户交易(T2)，如何让这两笔交易互不影响？ I

- 如果交易完成时数据库突然崩溃，如何保证交易数据成功保存在数据库中？ D
- 如何在支持大量交易的同时，保证数据的合法性(没有钱凭空产生或消失) ？ C



要保证交易正常可靠地进行，数据库就得解决上面的四个问题，这也就是`事务`诞生的背景，它能解决上面的四个问题，对应地，它拥有四大特性（**ACID**）。

![img](Redis.assets/1605696238342-934197cd-42f1-4e73-8f14-d6231fb68656.jpeg)

**（1）原子性（****A****tomicity）: 事务要么全部完成，要么全部取消。 如果事务崩溃，状态回到事务之前（事务回滚）。**

确保不管交易过程中发生了什么意外状况（服务器崩溃、网络中断等），不能出现A账户少了一个亿，但B账户没到帐，或者A账户没变，但B账户却凭空收到一个亿（数据不一致）。A和B账户的金额变动要么同时成功，要么同时失败(保持原状)。



**（2）隔离性（Isolation）: 如果2个事务 T1 和 T2 同时运行，事务 T1 和 T2 最终的结果是相同的，不管 T1和T2谁先结束。**

如果A在转账1亿给B（T1），同时C又在转账3亿给A（T2），不管T1和T2谁先执行完毕，最终结果必须是A账户增加2亿，而不是3亿，B增加1亿，C减少3亿。



**（3）持久性（Durability）: 一旦事务提交，不管发生什么（崩溃或者出错），数据要保存在数据库中。**

确保如果 T1 刚刚提交，数据库就发生崩溃，T1执行的结果依然会保持在数据库中。



**（4）一致性（Consistency）: 只有合法的数据（依照关系约束和函数约束）才能写入数据库。**

确保钱不会在系统内凭空产生或消失， 依赖原子性和隔离性。



可以看出，原子性、隔离性、一致性的根本问题，是不同的事务同时对同一份数据(A账户)进行`写操作`(修改、删除、新增)，如果事务中都只是读数据的话，那么它们可以随意地同时进行，反正读到的数据都是一样的。



如果，几个互不知晓的事务在同时修改同一份数据，那么很容易出现后完成的事务覆盖了前面的事务的结果，导致不一致。 事务在最终提交之前都有可能会回滚，撤销所有修改：



- 如果T1事务修改了A账户的数据，
- 这时T2事务读到了更新后的A账户数据，并进行下一步操作，

- 但此时T1事务却回滚了，撤销了对A账户的修改，
- 那么T2读取到的A账户数据就是非法的，这会导致数据不一致。



这些问题都是事务需要避免的。



### Redis 中的事务

Redis 中提供了以下三个命令来处理事务：

```shell
# 标记一个事务块的开始
# 事务块内的多条命令会按照先后顺序被放进一个队列当中，最后由 EXEC 命令原子性(atomic)地执行
MULTI

# 执行所有事务块内的命令。
EXEC

# 取消事务，放弃执行事务块内的所有命令。
DISCARD
```



示例：

```shell
SET Jack 10

SET Rose 20

# Jack 给 Rose 转账 5 块钱

# 开启事务
MULTI

DECRBY Jack 5

INCRBY ROSE 5

EXEC
```

上面的代码演示了事务的使用方式。



**（1）开始事务**：首先使用 `MULTI` 命令告诉 Redis：“下面我发给你的命令属于同一事务，你先不要执行，而是把它们暂时存起来”。Redis 回答：“OK”



**（2）命令入队**：而后我们发送了两个命令来实现相关操作，可以看到 Redis 遵守了承诺，没有执行这些命令，而是返回 `QUEUED` 表示这两条命令已经进入等待执行的事务队列中了



**（3）执行事务**：当把所有要在同一事务中执行的命令都发给 Redis 后，我们使用 `EXEC` 命令告诉 Redis 将等待执行的事务队列中的所有命令按照发送的顺序依次执行。`EXEC` 命令的返回值就是这些命令的返回值组成的列表，返回值顺序和命令的顺序相同。



（4）如果想要**取消事务**，则执行 `DISCARD` 命令。



Redis 保证了一个事务中的所有命令要么都执行，要么都不执行。如果在发送 `EXEC` 命令前客户端掉线了，则 Redis 会清空事务队列，事务中的所有命令都不会执行。而一旦客户端发送了 `EXEC` 命令，所有的命令就都会被执行，即使此后客户端断线也没关系，因为 Redis 中已经记录了所有要执行的命令。



除此之外，Redis 的事务还能保证一个事务内的命令依次执行而不被其它命令插入。试想客户端 A 需要执行几条命令，同时客户端 B 发送了一条命令，如果不适用事务，则客户端 B 的命令可能会插入到客户端 A 的几条命令中执行。如果不希望发送这种情况，也可以使用事务。



### 事务中的错误处理

如果一个事务中的某个命令执行出错，Redis 会怎么处理呢？要回答这个问题，首先需要知道什么原因导致命令执行出错。



（1）语法错误。语法错误指命令不存在或命令参数的个数不对。比如：

```shell
MULTI

# 正确的命令
SET key value

# 错误的命令
SET key

ERRORCOMMAND key

EXEC
```

跟在 MULTI 命令后执行了 3 个命令：

- 一个正确的命令，成功的加入了事务队列
- 其余两个命令都有语法错误

而只要有一个命令有语法错误，执行 `EXEC` 命令后 Redis 就会直接返回错误，连语法正确的命令也不会执行。





（2）运行错误。运行错误指在命令执行时出现的错误，比如使用散列类型的命令操作集合类型的键，这种错误在实际执行之前 Redis 是无法发现的，所以在事务里这样的命令是会被 Redis 接受并执行的。如果事务里的一条命令出现了运行错误，事务里其它的命令依然会继续执行，例如：

```shell
MULTI

SET key 1

SADD key 2

SET key 3

EXEC
```

可见虽然 `SADD key 2` 出现了错误，但是 `SET key 3` 依然执行了。



Redis 事务没有关系数据库事务提供的回滚（rollback）功能。为此开发者必须在事务执行出错后自己收拾剩下的摊子（将数据库复原回事务执行前的状态等）。



不过由于 Redis 不支持回滚功能，也使得 Redis 在事务上可以保持简洁和快速。此外回顾刚才提到的会导致事务执行失败的两种错误，其中语法错误完全可以在开发时找出并解决，另外如果能够很好的规划数据库的使用，是不会出现如命令与数据类型不匹配这样的运行时错误的。



### 事务中的 WATCH 命令

关于 WATCH 命令，我们来一个生活中的例子比较好理解。



假设我的银行卡有 100 元，此时我去商店买东西：

```shell
# 开启事务
MULTI

# 假设里面有 100 元
SET balance 100

# 拿了瓶水
DECRBY balance 3

# 拿了包烟
DECRBY balance 20
```

我的银行卡除了我自己消费使用，还绑定了我媳妇儿的支付宝，如果我在消费的时候，她也消费了会怎么样？

```shell
# 开启事务
MULTI

# 买了 10 斤苹果
DECRBY balance 100

EXEC
```

这时候我媳妇在超市直接刷了 100，此时余额不足的我还在挑口香糖...



针对于上面的场景，我们可以使用 Redis 事务中提供的 WATCH 功能来解决这个问题。



WATCH 定义：监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。



WATCH 相关命令如下：

```shell
# 监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。
WATCH key [key ...]

# 取消 WATCH 命令对所有 key 的监视。
# 如果在执行 WATCH 命令之后， EXEC 命令或 DISCARD 命令先被执行了的话，那么就不需要再执行 UNWATCH 了。
UNWATCH
```



使用示例：

```shell
SET balance 100

WATCH balance

DECRBY balance 30

MULTI

DECRBY balance 10

EXEC

GET balance # 70
```

如果在执行 WATCH 命令之后， EXEC 命令或 DISCARD 命令被执行了的话，那么会自动取消 WATCH。



如果需要手动停止 WATCH 则可以可以使用 UNWATCH 命令，UNWATCH 命令会取消 WATCH 命令对所有 key 的监视。



### 参考链接

- https://zhuanlan.zhihu.com/p/43493165
- https://xie.infoq.cn/article/84baa7fa9c2c3d3698a601def



## Redis 持久化

Redis 的强劲性能很大程度上是由于其将所有数据都存储在内存中，然而当 Redis 重启或宕机后，所有存储在内存中的数据就会丢失。在一些情况下，我们会希望 Redis 在重启后能够保证数据不丢失。

这时我们希望 Redis 能将数据从内存中以某种形式同步到硬盘中，使得重启后可以根据硬盘中的记录恢复数据。这一过程就是持久化。

Redis 提供了两种持久化方案：

- RDB 持久化，根据指定的规则“定时”将内存中的数据存储在硬盘上，在重启之后读取硬盘上的 `.rdb` 快照文件将数据恢复到内存中。
- AOF 持久化：AOF 持久化记录服务器执行的所有写操作命令形成 `.aof` 日志文件保存到硬盘中，并在服务器启动时，通过重新执行这些命令来还原数据集。



### RDB 持久化

RDB 方式的持久化是通过快照完成的，当符合一定条件时 Redis 会自动将内存中的所有数据生成一份副本并存储在硬盘上，这个过程即为“快照”。



Redis 允许用户自定义快照条件，当符合快照条件时，Redis 会自动执行快照操作。进行快照的条件可以由用户在配置文件中自定义，由两个参数构成：时间窗口 M 和改动的键的个数  N。每当时间 M 内被更改的键的个数大于 N 时，即符合自动快照条件。



RDB 持久化相关配置规则如下：

```shell
save 900 1 # 每 900 秒至少有 1 个 key 变化了，则写入快照
save 300 10 # 每 300 秒至少有 10 个 key 变化了，则写入快照
save 60 10000 # 每 60 秒至少有 10000 个 key 变化了，则写入快照

dbfilename dump.rdb # 快照保存的文件名称

dir ./ # 快照文件保存路径
```



RDB 快照的运行方式是异步进行的，在保存快照期间依然能够提供客户端请求。

![img](Redis.assets/1605767732245-ef3bdfd9-3ffd-4ab5-a418-beba62ae5c2c-2749481.png)

1. Redis 调用 `fork()` 创建一个子进程
2. 使用子进程将数据集写入到一个临时 RDB 文件中

1. 当子进程完成对新 RDB 文件的写入时，Redis 用新 RDB 文件替换原来的 RDB 文件，并删除旧的 RDB 文件



### AOF 持久化

快照功能并不是非常持久（durable）： 如果 Redis 因为某些原因而造成故障停机， 那么服务器将丢失最近写入、且仍未保存到快照中的那些数据。



尽管对于某些程序来说， 数据的持久性并不是最重要的考虑因素， 但是对于那些追求完全持久化（full durability）的程序来说， 快照功能就不太适用了。



从 1.1 版本开始， Redis 增加了一种完全耐久的持久化方式： AOF 持久化。



AOF 可以将 Redis 执行的每一条写命令操作日志存储到硬盘文件中，这一过程显然会降低 Redis 的性能，但是大部分情况下这个影响是可以接受的，另外使用较快硬盘可以提供 AOF 性能。



AOF 机制对于日志的写入操作采用的是 `append` 模式，就是追加模式，因此在写入过程中如果出现宕机问题，也不会破坏已经写入的日志数据。



默认情况下，Redis 没有开启 AOF 方式的持久化，可以通过 `appendonly` 参数启用：

```shell
appendonly yes
```

AOF 文件的保存位置和 RDB 文件的位置相同，都是 `dir` 参数设置的。默认的文件名是 `appendonly.aof`，可以通过 `appendfilename` 来修改：

```shell
# AOF 文件和 RDB 文件保存目录是一样的
dir ./

# 同步的文件名称
appendfilename "appendonly.aof"
```



AOF 有三种同步策略：

```shell
# 每修改同步，每一次发送数据变化都会被立即同步到磁盘中，效率比较低，但是数据最安全
appendfsync always

# 默认值，每秒同步，异步完成，同步效率非常高，缺点是一旦系统出现宕机，这1秒之内操作的数据就会丢失
appendfsync everysec

# 不同步
appendfsync no
```

一般情况下使用默认值 everysec 就足够了，既兼顾了性能又保证了安全。



### RDB vs AOF

参考阅读：http://doc.redisfans.com/topic/persistence.html。

| 持久化方式 | 优点                                            | 缺点                                                         |
| ---------- | ----------------------------------------------- | ------------------------------------------------------------ |
| RDB        | 文件小异步备份，性能好恢复大数据集速度比 AOF 快 | 数据安全性低，容易丢失数据数据量比较大时备份速度慢           |
| AOF        | 数据安全性高有利于开发分析                      | 相同数据集比 RDB 文件大根据所使用的 fsync 策略，AOF 速度可能会慢于 RDB |

一般来说， 如果想达到足以媲美关系型数据库的安全性， 你应该同时使用两种持久化功能。



如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。



有很多用户都只使用 AOF 持久化， 但我们并不推荐这种方式： 因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份， 并且 RDB 恢复数据集的速度也要比 AOF 恢复的速度要快。



### 参考链接

- http://doc.redisfans.com/topic/persistence.html



## 使用编程语言客户端操作 Redis

在此之前我们进行的操作都是通过 Redis 的命令行客户端 redis-cli 进行的，并没与介绍实际编程时如何操作 Redis。接下来主要会讲解如何通过具体的编程语言来操作 Redis。



### Redis 支持的程序客户端

参考官方支持列表：https://redis.io/clients。



### 在 Node.js 中操作 Redis

Node.js 中可以操作 Redis 的软件包推荐列表：https://redis.io/clients#nodejs。



推荐下面两个：

- https://github.com/NodeRedis/node-redis
- https://github.com/luin/ioredis



这里我主要以 ioredis 为例。



ioredis 是功能强大的 Redis 客户端，已被世界上最大的在线商务公司阿里巴巴和许多其他了不起的公司所使用。



ioredis 特点：

- 功能齐全。它支持集群，前哨，流，流水线，当然还支持Lua脚本和发布/订阅（具有二进制消息的支持）。
- 高性能

- 令人愉快的 API。它的异步 API 支持回调函数与 Promise
- 命令参数和返回值的转换

- 透明键前缀
- Lua脚本的抽象，允许您定义自定义命令。

- 支持二进制数据
- 支持 TLS

- 支持脱机队列和就绪检查
- 支持ES6类型，例如 Map 和 Set

- 支持GEO命令（Redis 3.2不稳定）
- 复杂的错误处理策略

- 支持NAT映射
- 支持自动流水线



相关链接：

- API 参考文档：https://github.com/luin/ioredis/blob/master/API.md
- 更新日志：https://github.com/luin/ioredis/blob/master/Changelog.md

- 从 node_redis 迁移：https://github.com/luin/ioredis/wiki/Migrating-from-node_redis



#### 基本使用

安装依赖：

```shell
npm install ioredis
const Redis = require("ioredis");
const redis = new Redis(); // uses defaults unless given configuration object

// ioredis supports all Redis commands:
redis.set("foo", "bar"); // returns promise which resolves to string, "OK"

// the format is: redis[SOME_REDIS_COMMAND_IN_LOWERCASE](ARGUMENTS_ARE_JOINED_INTO_COMMAND_STRING)
// the js: ` redis.set("mykey", "Hello") ` is equivalent to the cli: ` redis> SET mykey "Hello" `

// ioredis supports the node.js callback style
redis.get("foo", function (err, result) {
  if (err) {
    console.error(err);
  } else {
    console.log(result); // Promise resolves to "bar"
  }
});

// Or ioredis returns a promise if the last argument isn't a function
redis.get("foo").then(function (result) {
  console.log(result); // Prints "bar"
});

// Most responses are strings, or arrays of strings
redis.zadd("sortedSet", 1, "one", 2, "dos", 4, "quatro", 3, "three");
redis.zrange("sortedSet", 0, 2, "WITHSCORES").then((res) => console.log(res)); // Promise resolves to ["one", "1", "dos", "2", "three", "3"] as if the command was ` redis> ZRANGE sortedSet 0 2 WITHSCORES `

// All arguments are passed directly to the redis server:
redis.set("key", 100, "EX", 10);
```

有关更多实例，可以参考这里：https://github.com/luin/ioredis/tree/master/examples。



#### Pipelining

如果要发送一批命令（例如> 5），则可以使用流水线将命令在内存中排队，然后将它们一次全部发送到 Redis。这样，性能提高了50％〜300％（请参阅基准测试部分）。



`redis.pipeline()` 创建一个 Pipeline 实例。您可以像 Redis 实例一样在其上调用任何 Redis 命令。这些命令在内存中排队，并通过调用 `exec` 方法刷新到 Redis：

```javascript
const pipeline = redis.pipeline();
pipeline.set("foo", "bar");
pipeline.del("cc");
pipeline.exec((err, results) => {
  // `err` is always null, and `results` is an array of responses
  // corresponding to the sequence of queued commands.
  // Each response follows the format `[err, result]`.
});

// You can even chain the commands:
redis
  .pipeline()
  .set("foo", "bar")
  .del("cc")
  .exec((err, results) => {});

// `exec` also returns a Promise:
const promise = redis.pipeline().set("foo", "bar").get("foo").exec();
promise.then((result) => {
  // result === [[null, 'OK'], [null, 'bar']]
});
```

每个链接的命令还可以具有一个回调，该回调将在命令得到答复时被调用：

```javascript
redis
  .pipeline()
  .set("foo", "bar")
  .get("foo", (err, result) => {
    // result === 'bar'
  })
  .exec((err, result) => {
    // result[1][1] === 'bar'
  });
```

除了将命令分别添加到管道队列之外，您还可以将命令和参数数组传递给构造函数：

```javascript
redis
  .pipeline([
    ["set", "foo", "bar"],
    ["get", "foo"],
  ])
  .exec(() => {
    /* ... */
  });
```

`#length` 属性显示管道中有多少个命令：

```javascript
const length = redis.pipeline().set("foo", "bar").get("foo").length;
// length === 2
```



#### 事务

大多数时候，事务命令 `multi＆exec` 与管道一起使用。因此，在调用 `multi` 时，默认情况下会自动创建 `Pipeline` 实例，因此您可以像使用管道一样使用 `multi`：

```javascript
redis
  .multi()
  .set("foo", "bar")
  .get("foo")
  .exec((err, results) => {
    // results === [[null, 'OK'], [null, 'bar']]
  });
```

如果事务的命令链中存在语法错误（例如，错误的参数数量，错误的命令名称等），则不会执行任何命令，并返回错误：

```javascript
redis
  .multi()
  .set("foo")
  .set("foo", "new value")
  .exec((err, results) => {
    // err:
    //  { [ReplyError: EXECABORT Transaction discarded because of previous errors.]
    //    name: 'ReplyError',
    //    message: 'EXECABORT Transaction discarded because of previous errors.',
    //    command: { name: 'exec', args: [] },
    //    previousErrors:
    //     [ { [ReplyError: ERR wrong number of arguments for 'set' command]
    //         name: 'ReplyError',
    //         message: 'ERR wrong number of arguments for \'set\' command',
    //         command: [Object] } ] }
  });
```

就接口而言，`multi` 与管道的区别在于，当为每个链接的命令指定回调时，排队状态将传递给回调，而不是命令的结果：

```javascript
redis
  .multi()
  .set("foo", "bar", (err, result) => {
    // result === 'QUEUED'
  })
  .exec(/* ... */);
```

如果要使用不带管道的事务，请将 `{ pipeline: false }` 传递给 `multi`，每个命令将立即发送到 Redis，而无需等待 `exec` 调用：

```javascript
redis.multi({ pipeline: false });
redis.set("foo", "bar");
redis.get("foo");
redis.exec((err, result) => {
  // result === [[null, 'OK'], [null, 'bar']]
});
```

`multi` 的构造函数还接受一批命令：

```javascript
redis
  .multi([
    ["set", "foo", "bar"],
    ["get", "foo"],
  ])
  .exec(() => {
    /* ... */
  });
```

管道支持内联事务，这意味着您可以将管道中的命令子集分组为一个事务：

```javascript
redis
  .pipeline()
  .get("foo")
  .multi()
  .set("foo", "bar")
  .get("foo")
  .exec()
  .get("foo")
  .exec();
```





#### 错误处理

Redis服务器返回的所有错误都是 `ReplyError` 的实例，可以通过 Redis 进行访问：

```javascript
const Redis = require("ioredis");
const redis = new Redis();
// This command causes a reply error since the SET command requires two arguments.
redis.set("foo", (err) => {
  err instanceof Redis.ReplyError;
});
```

这是 `ReplyError` 的错误堆栈：

```plain
ReplyError: ERR wrong number of arguments for 'set' command
    at ReplyParser._parseResult (/app/node_modules/ioredis/lib/parsers/javascript.js:60:14)
    at ReplyParser.execute (/app/node_modules/ioredis/lib/parsers/javascript.js:178:20)
    at Socket.<anonymous> (/app/node_modules/ioredis/lib/redis/event_handler.js:99:22)
    at Socket.emit (events.js:97:17)
    at readableAddChunk (_stream_readable.js:143:16)
    at Socket.Readable.push (_stream_readable.js:106:10)
    at TCP.onread (net.js:509:20)
```

默认情况下，错误堆栈没有任何意义，因为整个堆栈都发生在 ioredis 模块本身而不是代码中。因此，要找出错误在代码中的位置并不容易。 ioredis 提供了一个选项 `showFriendlyErrorStack` 来解决该问题。启用 `showFriendlyErrorStack` 时，ioredis 将为您优化错误堆栈：

```javascript
const Redis = require("ioredis");
const redis = new Redis({ showFriendlyErrorStack: true });
redis.set("foo");
```

输出将是：

```plain
ReplyError: ERR wrong number of arguments for 'set' command
    at Object.<anonymous> (/app/index.js:3:7)
    at Module._compile (module.js:446:26)
    at Object.Module._extensions..js (module.js:464:10)
    at Module.load (module.js:341:32)
    at Function.Module._load (module.js:296:12)
    at Function.Module.runMain (module.js:487:10)
    at startup (node.js:111:16)
    at node.js:799:3
```

这次，堆栈告诉您错误发生在代码的第三行。

太好了！但是，优化错误堆栈会大大降低性能。因此，默认情况下，此选项是禁用的，只能用于调试目的。不建议在生产环境中使用此功能。



## Redis 设置远程连接

注意：为了保护数据安全，开放远程连接需谨慎操作。

Redis 默认是不允许远程连接的，通过下面的配置可以开启远程连接。

将 `redis.conf` 配置文件的 `bind` 和 `protected-mode` 修改如下：

```shell
# 绑定的端口号
bind 0.0.0.0

# 关闭保护模式
protected-mode no
```

除此之外还需要检查服务器防火墙是否开放了 Redis 服务占用的端口号。

修改之后重启 Redis 服务即可生效。
