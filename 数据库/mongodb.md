[TOC]

# MongoDB

## NoSQL 简介

### 关系型数据库遇到的问题

2008 年左右，网站 、 论坛、社交网络开始高速发展，传统的关系型数据库在存储及处理数据的时候受到了很大的挑战 ，其中主要体现在以下几点：

- 难以应付每秒上万次的高并发数据写入 。
- 查询上亿量级数据的速度极其缓慢 。

- 分库、分表形成的子库到达一定规模后难以进一步扩展 。
- 分库、分表 的规则可能会因为需求变更而发生变更。

- 修改表结构困难 。

在很多 互联网应用场景下 ， 对数据联表的查询需求不是那么强烈 ，也并不需要在数据写入后立刻读取，但对数据的读取和并发写入速度有非常高的要求 。 在这样的情况下 ，非关系型数据库得到高速的发展 。



### 什么是 NoSQL 数据库

如果你之前只接触过关系型数据库如 Oracle、Mysql 或 SQL Server，在学习 MongoDB 时可能会感到不安，突然有一款数据库不支持外键，不支持事务，不支持数据类型约定，会给人一种没法用的感觉。

MongoDB 就是这样一款非关系型的数据库，什么叫非关系型？就是把数据直接放进一个大仓库，不标号、不连线、单纯的堆起来。传统数据库由于受到各种关系的累赘，各种数据形式的束缚，难以处理海量数据以及超高并发的业务场景。

为了解决上述问题，必须有一款自废武功，以求在更高层次上突破瓶颈的数据库系统。就像张无忌忘记招式从而学习太极一样，摈弃了固有模式的 MongoDB 才能应对 Facebook 上亿比特的海量数据。

NoSQL(NoSQL = Not Only SQL )，意即"不仅仅是SQL"。

在现代的计算系统上每天网络上都会产生庞大的数据量，这些数据有很大一部分是由关系数据库管理系统（RDBMS）来处理。

1970年 E.F.Codd's提出的关系模型的论文 "A relational model of data for large shared data banks"，这使得数据建模和应用程序编程更加简单。

通过应用实践证明，关系模型是非常适合于客户服务器编程，远远超出预期的利益，今天它是结构化数据存储在网络和商务应用的主导技术。

NoSQL 是一项全新的数据库革命性运动，早期就有人提出，发展至2009年趋势越发高涨。NoSQL的拥护者们提倡运用非关系型的数据存储，相对于铺天盖地的关系型数据库运用，这一概念无疑是一种全新的思维的注入。



### NoSQL 数据库有哪些特点

- 可弹性扩展
- BASE 特性

- 大数据量、高性能
- 灵活的数据模型

- 高可用



### NoSQL 数据库有哪些种类

#### 键值数据库

这类数据库主要是使用数据结构中的键 Key 来查找特定的数据Value。

- 优点：在存储时不采用任何模式，因此极易添加数据

这类数据库具有极高的读写性能，用于处理大量数据的高访问负载比较合适。

键值对数据库适合大量数据的高访问及写入负载场景，例如日志系统。

主要代表是 Redis、Flare。



#### 文档型数据库

这类数据库满足了海量数据的存储和访问需求，同时对字段要求不严格，可以随意增加、删除、修改字段，且不需要预先定义表结构，所以适用于各种网络应用。

主要代表是 MongoDB、CouchDB。



#### 列存储型数据库

主要代表是 Cassandra 、Hbase。

这类数据库查找速度快，可扩展性强，适合用作分布式文件存储系统。



#### 图数据库

主要代表是 InfoGrid 、Neo4J 。

这类数据库利用“图结构”的相关算法来存储实体之间的关系信息，适合用于构建社交网络和推荐系统的关系图谱。



### NoSQL 与 RDB 该怎么选择

既然 NoSQL 数据库有这么多的优势，那它是否可以直接取代关系型数据库？

NoSQL 并不能完全取代关系型数据库，NoSQL 主要被用来处理大量且多元数据的存储及运算问题。在这样的特性差异下，我们该如何选择合适的数据库以解决数据存储与处理问题呢？这里提供以下几点作为判断依据。

**1、数据模型的关联性要求**

NoSQL 适合模型关联性比较低的应用。因此：

- 如果需要多表关联，则更适合用 RDB
- 如果对象实体关联少，则更适合用 NoSQL 数据库

- - 其中 MongoDB 可以支持复杂度相对高的数据结构，能够将相关联的数据以文档的方式嵌入，从而减少数据之间的关联操作

**2、数据库的性能要求**

如果数据量多切访问速度至关重要，那么使用 NoSQL 数据库可能是比较合适的。NoSQL 数据库能通过数据的分布存储大幅地提供存储性能。

**3、数据的一致性要求**

NoSQL 数据库有一个缺点：其在事务处理与一致性方面无法与 RDB 相提并论。

因此，NoSQL 数据库很难同时满足强一致性与高并发性。如果应用对性能有高要求，则 NoSQL 数据库只能做到数据最终一致。

**4、数据的可用性要求**

考虑到数据不可用可能会造成风险，NoSQL 数据库提供了强大的数据可用性（在一些需要快速反馈信息给使用者的应用中，响应延迟也算某种程度的高可用）。

**一个项目并非只选择一种数据库，可以将其拆开设计，将需要 RDB 特性的放到 RDB 中管理，而其它数据放到 NoSQL 中管理。**





## MongoDB 简介

![image.png](MongoDB.assets/1604328119528-7919816e-6178-47e3-b775-0d9bfed5666b.png)



### 什么是 MongoDB

- 官方文档：https://www.mongodb.com/
- MongoDB 是由 C++ 语言编写的，是一个基于分布式文件存储的开源 NoSQL 数据库系统。

- MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

- - 这会让曾经使用过关系型数据库的人比较容易上手

- MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

![img](MongoDB.assets/1604459344700-418329ca-3c73-4aa7-8ecf-c6cf93eb625d.png)

- MongoDB 的查询功能非常强大

- - 不仅支持大部分关系型数据库中的单表查询，还支持范围查询、排序、聚合、MapReduce 等
  - MongoDB 的查询语法类似于面相对象的程序语言



### MongoDB 有哪些特点

- 文档型数据库
- 高性能

- 灵活性
- 可扩展性

- 强大的查询语言
- 优异的性能

- 高性能：支持使用嵌入数据时，减少系统I/O负担，支持子文档查询
- 多种查询类型支持，且支持数据聚合查询、文本检索、地址位置查询

- 高可用、水平扩展：支持副本集与分片
- 多种存储引擎：WiredTiger , In-Memory



### MongoDB 发展历史

- 2007年10月，MongoDB 由 10gen 团队所发展
- 2009年2月首度推出 1.0 版

- 2011年9月，发布 2.0 版本

- - 分片、复制等功能

- 2015年3月，发布 3.0 版本

- - WiredTiger存储引擎支持

- 2018年6月，发布 4.0 版本

- - 推出ACID事务支持，成为第一个支持强事务的NoSQL数据库；

- ……



### MongoDB 适用于哪些场景

**1、需要处理大量的低价值数据，且对数据处理性能有较高要求**

比如，对微博数据的处理就不需要太高的事务性，但是对数据的存取性能有很高的要求，这时就非常适合使用 MongoDB。

**2、需要借助缓存层来处理数据**

因为 MongoDB 能高效的处理数据，所以非常适合作为缓存层来使用。将 MongoDB 作为持久化缓存层，可以避免底层存储的资源过载。

**3、需要高度的伸缩性**

对关系型数据库而言，当表的大小达到一定数量级后，其性能会急剧下降。这时可以使用多台 MongoDB 服务器搭建一个集群环境，实现最大程度的扩展，且不影响性能。





## 安装

### 在 macOS 中安装 MongoDB

#### 安装说明

- 关于 MongoDB 的版本号

- - 奇数为开发版（4.3），建议开发环境使用
  - 偶数为稳定版（4.4），建议生产环境使用

- 从版本 3.2 之后不再支持 32 位操作系统
- 课程中使用到的版本是最新稳定版 4.4

- - 仅支持 MacOS 10.13 及更高版本



在 macOS 上安装 MongoDB 有两种方式：

- 使用 [Homebrew](https://brew.sh/) 安装 MongoDB
- 下载 .tgz 压缩包手动安装



建议尽可能使用 Homebrew 包管理器安装 MongoDB。使用程序包管理器会自动安装所有必需的依赖项，并简化以后的升级和维护任务。



#### 使用 homebrew 安装 MongoDB

如果已经安装了 Homebrew 的可以跳过前两步。

1、安装 Command Line Tools for Xcode

如果你电脑上安装了 XCode 软件开发工具（在App Store中安装Xcode），Command Line Tools for Xcode已经给你安装好了。

也可以直接安装 Command Line Tools for Xcode。在终端输入以下代码完成安装：

```shell
xcode-select --install
```



2、安装 [Homebrew](https://brew.sh/)

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```



3、添加 MongoDB 安装源到 Homebrew

```shell
brew tap mongodb/brew
```



4、使用 homebrew 安装 MongoDB

```shell
brew install mongodb-community@4.4
```

该安装除安装必要的二进制文件之外，还会创建运行 MongoDB 服务所需的文件目录：

- MongoDB 配置文件：`/usr/local/etc/mongod.conf`
- 日志文件存储目录：`/usr/local/var/log/mongodb`

- 数据文件存储目录：`/usr/local/var/mongodb`



#### 管理 MongoDB 服务



##### 启动 MongoDB

启动 MongoDB 并运行在后台。

```shell
brew services start mongodb-community@4.4
```



或者手动启动 MongoDB，运行在前台。也可以加入 --fork 参数运行在后台。

```shell
mongod --config /usr/local/etc/mongod.conf
```



##### 查看 MongoDB 服务运行状态

要验证MongoDB是否正在运行，请在正在运行的进程中搜索 mongod：

```shell
ps aux | grep -v grep | grep mongod
```

还可以通过查看日志文件以查看 mongod 进程的当前状态：`/usr/local/var/log/mongodb/mongo.log`。



##### 停止 MongoDB

```shell
brew services stop mongodb-community@4.4
```



##### 卸载 MongoDB

```shell
brew uninstall mongodb-community@4.4
```





### 在 Windows 中安装 MongoDB

#### 安装说明

- 关于 MongoDB 的版本号

- - 奇数为开发版（4.3），建议开发环境使用
  - 偶数为稳定版（4.4），建议生产环境使用

- 从版本 3.2 之后不再支持 32 位操作系统
- 课程中使用到的版本是最新稳定版 4.4

- MongoDB 4.4 支持的 Windows 64 位系统版本

- - Windows Server 2019
  - Windows 10 / Windows Server 2016



#### 安装

1、下载安装包

2、运行安装程序

![img](MongoDB.assets/1604295244059-3e4616d1-671e-46a9-99f0-c2888b929d7f.png)

点击 Next 下一步

![img](MongoDB.assets/1604295262767-8cf2de39-250e-46c4-a45f-b98023a277a4.png)

勾选同意协议，点击 Next 下一步

![img](MongoDB.assets/1604295335348-6557f677-5ef9-4b7f-9b40-f4c6e819b0b1.png)

这里选择安装方式：

- Complete：完整安装
- Custom：自定义安装

我这里选择 Complete 完整安装。

![img](MongoDB.assets/1604295389658-bf91176d-c3d9-48ec-8ddc-1a0b6081eaef.png)

从 MongoDB 4.0 开始，可以在安装过程中将 MongoDB 设置为 Windows 服务，也可以仅安装二进制文件。

Service Name：服务名称

Data Directory：数据存储位置

Log Directory：日期存储位置

![img](https://cdn.nlark.com/yuque/0/2020/png/152778/1604295701912-b080d39a-5458-4e15-bef5-98c802b75048.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_5ouJ5Yu-5pWZ6IKy%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

MongoDB Compass 是 MongoDB 官网提供的一个集创建数据库、管理集合和文档、运行临时查询、评估和优化查询、性能图表、构建地理查询等功能为一体的MongoDB可视化管理工具。

不建议在这里勾选安装，因为它要在线下载然后安装，比较耗时。如果需要，可以单独去官网下载安装。

![img](MongoDB.assets/1604295711062-cfa685c4-d8af-446a-8225-1ba6c336fd94.png)

点击 Install 开始安装

![img](MongoDB.assets/1604295760738-46ea113f-9a60-4962-9add-ebd00e032073.png)

安装中...

![img](MongoDB.assets/1604295780058-e7bf5c58-2f82-4a2d-aa80-0652f37eaa88.png)

安装完成，点击 Finish 结束安装



#### 管理 MongoDB 服务

##### 作为 Windows 服务的启动和停止

如果你将 MongoDB 安装为 Windows 的服务了，则 MongoDB 已经自动启动了，并且默认会开启启动。

（1）打开 Windows 服务控制台

打开运行，输入 `services.msc` 打开。



（2）启动、停止、暂停、恢复、重新启动

![img](MongoDB.assets/1604302685844-9500b6be-a049-45ac-b0d3-b48acae3512f.png)



##### 使用命令方式启动和停止

如果没有把 MongoDB 安装 Windows 服务，则需要按照下面的方式来启动 MongoDB。

1、将 MongoDB 安装目录中的 bin 目录配置到系统 PATH 环境变量

2、创建数据存储目录

MongoDB 的默认数据目录路径是启动 MongoDB 的驱动器上的绝对路径 `\data\db`。

```shell
cd c:\
md "\data\db"
```

3、启动 MongoDB 服务

```shell
mongod
```

`--dbpath` 选项用来指定数据存储目录，默认使用 MongoDB 程序所在驱动器上的绝对路径 `\data\db`。



MongoDB默认端口是27017：

![image-20210427164403409](MongoDB.assets/image-20210427164403409.png)

mongod 命令是启用数据库服务，即搭建并开启服务器，可以通过端口被访问（27017）

mongo 命令是连接数据库服务，即连接服务器，可以通过端口进行访问（27017）

注意：mongod 和 mongo 的区别，前者是启用MongoDB进程，后者是对MongoDB进行连接操作。



## 备份/恢复

**备份：**

```bash
mongodump -h localhost -u root -p 密码 -d test -o /tmp/test
```

参数：

- -h：MongoDB 所在服务器地址
- -u：用户名
- -p：密码
- -d：需要备份的数据库，例如：test，不传-d则备份所有数据库。
- -o：备份的数据存放位置。

备注：可以使用docker cp将容器内部的目录复制到宿主机的目录，从而实现将备份数据保存到宿主机目录。

```bash
docker cp 容器id:容器目录 宿主机目录
```



**恢复：**

```bash
mongorestore -h localhost -u root -p 密码 -d test --dir /tmp/test
```

参数：

- -h：MongoDB 所在服务器地址
- -u：用户名
- -p：密码
- -d：需要备份的数据库，例如：test，不传-d则备份所有数据库。
- --dir：指定备份存放的目录。



## mongo Shell

### 什么是 mongo Shell

- mongo Shell 是 MongoDB 官方提供的一个在命令行中用来连接操作 MongoDB 服务的客户端工具
- 使用 mongo Shell 可以对 MongoDB 数据库进行数据的管理



### 下载 mongo Shell

mongo Shell 包含在 MongoDB 服务器安装中。如果您已经安装了服务器，则 mongo Shell 将安装在与服务器二进制文件相同的位置。

另外，如果您想从 MongoDB 服务器上单独下载 mongo shell，可以参考这里：https://docs.mongodb.com/manual/mongo/#download-the-mongo-shell。



### 启动 mongo Shell 并连接到 MongoDB

#### 连接默认端口上的本地 MongoDB 服务

您可以在没有任何命令行选项的情况下运行 mongo shell，以使用默认端口 `27017` 连接到在本地主机上运行的 MongoDB 实例： 

```shell
mongo
```



#### 连接非默认端口上的本地 MongoDB 服务

要明确指定端口，请包括 `--port` 命令行选项。例如，要使用非默认端口 `28015` 连接到在 `localhost` 上运行的 MongoDB 实例，请执行以下操作：

```shell
mongo --port 28015
```



#### 连接远程主机上的 MongoDB 服务

连接远程主机上的 MongoDB 服务需要明确指定主机名和端口号。

您可以指定一个连接字符串。例如，要连接到在远程主机上运行的 MongoDB 实例，请执行以下操作： 

```shell
mongo "mongodb://mongodb0.example.com:28015"
```



您可以使用命令行选项 `--host <主机>:<端口>`。例如，要连接到在远程主机上运行的 MongoDB 实例，请执行以下操作： 

```shell
mongo --host mongodb0.example.com:28015
```



您可以使用--host <host>和--port <port>命令行选项。例如，要连接到在远程主机上运行的MongoDB实例，请执行以下操作： 

```shell
mongo --host mongodb0.example.com --port 28015
```



#### 连接具有身份认证的 MongoDB 服务

您可以在连接字符串中指定用户名，身份验证数据库以及可选的密码。例如，以alice用户身份连接并认证到远程MongoDB实例：

```shell
mongo "mongodb://alice@mongodb0.examples.com:28015/?authSource=admin"
```



您可以使用--username <user>和--password，--authenticationDatabase <db>命令行选项。例如，以alice用户身份连接并认证到远程MongoDB实例：

```shell
mongo --username alice --password --authenticationDatabase admin --host mongodb0.examples.com --port 28015
```

注意：如果您指定--password而不输入用户密码，则外壳程序将提示您输入密码。



### mongo Shell 执行环境

- 提供了 JavaScript 执行环境
- 内置了一些数据库操作命令

- - show dbs
  - db

- - use database
  - show collections

- - ...

- 提供了一大堆的内置 API 用来操作数据库

- - db.users.insert({ name: 'Jack', age: 18 })



### 退出连接

三种方式：

- exit
- quit()

- Ctrl + C



## MongoDB 基础概念

到目前为止，我们已经学习了：

- 安装 MongoDB
- 启动 MongoDB 数据库服务

- 使用 mongo Shell 连接 MongoDB



接下来我们主要学习的内容就是如何管理 MongoDB 中的数据，但是在具体的操作之前要先来了解一下 MongoDB 数据库中的一些基本概念。

- MongoDB 中的数据存储结构
- 数据库

- 集合
- 文档



### MongoDB 中的数据存储结构

由于 MongoDB 是文档型数据库，其中存储的数据就是熟悉的 JSON 格式数据。

- 你可以把 MongoDB 数据库想象为一个超级大对象
- 对象里面有不同的集合

- 集合中有不同的文档

```json
{
  // 数据库 Database
  "京东": {
    // 集合 Collection，对应关系型数据库中的 Table
    "用户": [
      // 文档 Document，对应关系型数据库中的 Row
      {
        // 数据字段 Field，对应关系数据库中的 Column
        "id": 1,
        "username": "张三",
        "password": "123"
      },
      {
        "id": 2,
        "username": "李四",
        "password": "456"
      }
      // ...
    ],
    "商品": [
      {
        "id": 1,
        "name": "iPhone Pro Max",
        "price": 100
      },
      {
        "id": 2,
        "name": "iPad Pro",
        "price": 80
      }
    ],
    "订单": []
    // ...
  },

  // 数据库
  "淘宝": {}

  // ...
}
```



### 数据库

在 MongoDB 中，数据库包含一个或多个文档集合。



#### 查看数据库列表

```shell
show dbs
```



#### 查看当前数据库

```shell
db
```



MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。

- **admin**：从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
- **local：** 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合

- **config：**当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。



#### 创建/切换数据库

```shell
use <DATABASE_NAME>
```

在 MongoDB 中数据库只有真正的有了数据才会被创建出来。

你可以切换到不存在的数据库。首次将数据存储在数据库中（例如通过创建集合）时，MongoDB 会创建数据库。例如，以下代码在 `insertOne()` 操作期间创建数据库 `myNewDatabase` 和集合 `myCollection`：

```javascript
use myNewDatabase
db.myCollection.insertOne( { x: 1 } );
```



#### 数据库名称规则

https://docs.mongodb.com/manual/reference/limits/#naming-restrictions

- 不区分大小写，但是建议全部小写
- 不能包含空字符。

- 数据库名称不能为空，并且必须少于64个字符。
- Windows 上的命名限制

- - 不能包括 `/\. "$*<>:|?` 中的任何内容

- Unix 和 Linux 上的命名限制

- - 不能包括 `/\. "$` 中的任何字符





#### 删除数据库

1、使用 use 命令切换到要删除的数据库

2、使用 `db.dropDatabase()` 删除当前数据库



### 集合

集合类似于关系数据库中的表，MongoDB 将文档存储在集合中。

![img](MongoDB.assets/1604488927220-2f3363e6-dc7a-4103-b745-895aa19b3af5.svg)



#### 创建集合

如果不存在集合，则在您第一次为该集合存储数据时，MongoDB 会创建该集合。

```javascript
db.myNewCollection2.insert( { x: 1 } )
```

MongoDB提供 `db.createCollection()` 方法来显式创建具有各种选项的集合，例如设置最大大小或文档验证规则。如果未指定这些选项，则无需显式创建集合，因为在首次存储集合数据时，MongoDB 会创建新集合。



#### 集合名称规则

集合名称应以下划线或字母字符开头，并且：

- 不能包含 `$`
- 不能为空字符串

- 不能包含空字符
- 不能以 `.` 开头

- 长度限制

- - 版本 4.2 最大 120 个字节
  - 版本 4.4 最大 255 个字节



#### 查看集合

```shell
show collections
```



#### 删除集合

```shell
db.集合名称.drop()
```



### 文档

- MongoDB 将数据记录存储为 BSON 文档
- BSON（Binary JSON）是 JSON 文档的二进制表示形式，它比 JSON 包含更多的数据类型

- [BSON 规范](http://bsonspec.org/)
- [BSON 支持的数据类型](https://docs.mongodb.com/manual/reference/bson-types/)

![img](MongoDB.assets/1604546663341-8e1f15b9-800d-4cf5-a5ef-29fa7fbe1d77.svg)



#### 文档结构

MongoDB 文档由字段和值对组成，并具有以下结构： 

```json
{
   field1: value1,
   field2: value2,
   field3: value3,
   ...
   fieldN: valueN
}
```



#### 字段名称

文档对字段名称有以下限制：

- 字段名称 `_id` 保留用作主键；它的值在集合中必须是唯一的，不可变的，并且可以是数组以外的任何类型。
- 字段名称不能包含空字符。

- 顶级字段名称不能以美元符号 `$` 开头。

- - 从 MongoDB 3.6 开始，服务器允许存储包含点 `.` 和美元符号 `$` 的字段名称



### MongoDB 中的数据类型

字段的值可以是任何 BSON 数据类型，包括其他文档，数组和文档数组。例如，以下文档包含各种类型的值：

```json
var mydoc = {
    _id: ObjectId("5099803df3f4948bd2f98391"),
    name: { first: "Alan", last: "Turing" },
    birth: new Date('Jun 23, 1912'),
    death: new Date('Jun 07, 1954'),
    contribs: [ "Turing machine", "Turing test", "Turingery" ],
    views : NumberLong(1250000)
}
```



上面的字段具有以下数据类型：

- _id 保存一个 [ObjectId](https://docs.mongodb.com/manual/reference/bson-types/#objectid) 类型
- name 包含一个嵌入式文档，该文档包含 first 和 last 字段

- birth 和 death 持有 Date 类型的值
- contribs 保存一个字符串数组

- views 拥有 NumberLong 类型的值



下面是 MongoDB 支持的常用数据类型。

| 类型               | 整数标识符 | 别名（字符串标识符） | 描述                                                         |
| ------------------ | ---------- | -------------------- | ------------------------------------------------------------ |
| Double             | 1          | “double”             | 双精度浮点值。用于存储浮点值。                               |
| String             | 2          | “string”             | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。 |
| Object             | 3          | “object”             | 用于内嵌文档                                                 |
| Array              | 4          | “array”              | 用于将数组或列表或多个值存储为一个键。                       |
| Binary data        | 5          | “binData”            | 二进制数据。用于存储二进制数据。                             |
| ObjectId           | 7          | “objectId”           | 对象 ID。用于创建文档的 ID。                                 |
| Boolean            | 8          | “bool”               | 布尔值。用于存储布尔值（真/假）。                            |
| Date               | 9          | “date”               | 日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。 |
| Null               | 10         | “null”               | 用于创建空值。                                               |
| Regular Expression | 11         | “regex”              | 正则表达式类型。用于存储正则表达式。                         |
| 32-bit integer     | 16         | “int”                | 整型数值。用于存储 32 位整型数值。                           |
| Timestamp          | 17         | “timestamp”          | 时间戳。记录文档修改或添加的具体时间。                       |
| 64-bit integer     | 18         | “long”               | 整型数值。用于存储 64 位整型数值。                           |
| Decimal128         | 19         | “decimal”            | 数值类型。常用于存储更精确的数字，例如货币。                 |



#### _id 字段

在 MongoDB 中，存储在集合中的每个文档都需要一个唯一的 `_id` 字段作为主键。如果插入的文档省略 `_id` 字段，则 MongoDB 驱动程序会自动为 `_id` 字段生成 `ObjectId`。

`_id` 字段具有以下行为和约束：

- 默认情况下，MongoDB 在创建集合时会在 `_id` 字段上创建唯一索引。
- `_id` 字段始终是文档中的第一个字段

- `_id` 字段可以包含任何 BSON 数据类型的值，而不是数组。





## 基础操作

### 创建文档

创建或插入操作将新文档添加到集合中。如果集合当前不存在，则插入操作将创建集合。



MongoDB 提供以下方法，用于将文档插入集合中：

| `db.collection.insertOne()`  | 插入单个文档到集合中        |
| ---------------------------- | --------------------------- |
| `db.collection.insertMany()` | 插入多个文档到集合中        |
| `db.collection.insert()`     | 将1个或多个文档插入到集合中 |

![img](MongoDB.assets/1604313944549-a98ee89a-a84d-4115-8d2a-9c32e1d93759.svg)



#### 插入单个文档

```javascript
db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```



#### 插入多个文档

```javascript
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
   { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
   { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
])
```



#### 插入行为

1、集合创建

如果该集合当前不存在，则插入操作将创建该集合。



2、_id 字段

在 MongoDB 中，存储在集合中的每个文档都需要一个唯一的 _id 字段作为主键。如果插入的文档省略 _id 字段，则 MongoDB 驱动程序会自动为 _id 字段生成 ObjectId。



### 查询文档

#### 基本查询

读取操作从集合中检索文档；即查询集合中的文档。 MongoDB提供了以下方法来从集合中读取文档：

- db.collection.find(query, projection)

- - query ：可选，使用查询操作符指定查询条件
  - projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

- db.collection.findOne()

![img](MongoDB.assets/1604313990615-962088f4-0f71-4413-97c6-f1e1577e5651-20220118173149680.svg)

有关示例，请参见：

- [Query Documents](https://docs.mongodb.com/manual/tutorial/query-documents/)
- [Query on Embedded/Nested Documents](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/)

- [Query an Array](https://docs.mongodb.com/manual/tutorial/query-arrays/)
- [Query an Array of Embedded Documents](https://docs.mongodb.com/manual/tutorial/query-array-of-documents/)

开始下面的练习之前先来生成一些测试数据：

```javascript
db.inventory.insertMany([
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" }
]);
```



##### 查询所有文档

```javascript
db.inventory.find( {} )
```

等价于 SQL 中的 `SELECT * FROM inventory` 语句。

格式化打印结果：

```javascript
db.myCollection.find().pretty()
```



##### 指定返回的文档字段

```javascript
db.inventory.find({}, {
	item: 1,
  qty: 1
})
```



##### 相等条件查询

```javascript
db.inventory.find( { status: "D" } )
```

等价于 SQL 中的 `SELECT * FROM inventory WHERE status = "D"` 语句。



##### 指定 AND 条件

以下示例检索状态为“ A”且数量小于（$ lt）30的清单集合中的所有文档：

```javascript
db.inventory.find( { status: "A", qty: { $lt: 30 } } )
```

该操作对应于以下SQL语句： 

```sql
SELECT * FROM inventory WHERE status = "A" AND qty < 30
```



##### 指定 OR 条件

使用 `$or` 运算符，您可以指定一个复合查询，该查询将每个子句与一个逻辑或连接相连接，以便该查询选择集合中至少匹配一个条件的文档。

下面的示例检索状态为 `A` 或数量小于 `$lt30` 的集合中的所有文档：

```javascript
db.inventory.find({
  $or: [
    { status: "A" },
    { qty: { $lt: 30 } }
  ]
})
```

该操作对应于以下 SQL 语句：

```sql
SELECT * FROM inventory WHERE status = "A" OR qty < 30
```



##### 指定 AND 和 OR 条件

在下面的示例中，复合查询文档选择状态为“ A”且qty小于（$ lt）30或item以字符p开头的所有文档：

```javascript
db.inventory.find({
  status: "A",
  $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
})
```

该操作对应于以下SQL语句：

```sql
SELECT * FROM inventory WHERE status = "A" AND ( qty < 30 OR item LIKE "p%")
```



##### 使用查询运算符指定条件

下面的示例从状态为“ A”或“ D”等于“库存”的清单集中检索所有文档：

```javascript
db.inventory.find( { status: { $in: [ "A", "D" ] } } )
```

该操作对应以下 SQL 语句：

```sql
SELECT * FROM inventory WHERE status in ("A", "D")
```

完成的查询运算符参考：https://docs.mongodb.com/manual/reference/operator/query/。



##### 查询运算符

参考：https://docs.mongodb.com/manual/reference/operator/query-comparison/

比较运算符：

| 名称   | 描述                       |
| ------ | -------------------------- |
| `$eq`  | 匹配等于指定值的值。       |
| `$gt`  | 匹配大于指定值的值。       |
| `$gte` | 匹配大于或等于指定值的值。 |
| `$in`  | 匹配数组中指定的任何值。   |
| `$lt`  | 匹配小于指定值的值。       |
| `$lte` | 匹配小于或等于指定值的值。 |
| `$ne`  | 匹配所有不等于指定值的值。 |
| `$nin` | 不匹配数组中指定的任何值。 |

逻辑运算符：

| 名称   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| `$and` | 将查询子句与逻辑连接，并返回与这两个子句条件匹配的所有文档。 |
| `$not` | 反转查询表达式的效果，并返回与查询表达式不匹配的文档。       |
| `$nor` | 用逻辑NOR连接查询子句，返回所有不能匹配这两个子句的文档。    |
| `$or`  | 用逻辑连接查询子句，或返回与任一子句条件匹配的所有文档。     |



#### 查询嵌套文档

##### 匹配嵌套文档

要在作为嵌入/嵌套文档的字段上指定相等条件，请使用查询过滤器文档 `{<field>: <value>}`，其中 `<value>` 是要匹配的文档。

例如，以下查询选择字段大小等于文档 `{h: 14, w: 21, uom: "cm"}` 的所有文档：

```javascript
db.inventory.find({
  size: { h: 14, w: 21, uom: "cm" }
})
```

整个嵌入式文档上的相等匹配要求与指定的 `<value>` 文档完全匹配，包括字段顺序。例如，以下查询与库存收集中的任何文档都不匹配：

```javascript
db.inventory.find({
  size: { w: 21, h: 14, uom: "cm" }
})
```



#### 查询嵌套字段

要在嵌入式/嵌套文档中的字段上指定查询条件，请使用点符号 `("field.nestedField")`。

注意：使用点符号查询时，字段和嵌套字段必须在引号内。



##### 在嵌套字段上指定相等匹配

以下示例选择嵌套在 size 字段中的 uom 字段等于 `"in"`  的所有文档：

```javascript
db.inventory.find({
  "size.uom": "in"
})
```



##### 使用查询运算符指定匹配项

查询过滤器文档可以使用查询运算符以以下形式指定条件：

```javascript
{ <field1>: { <operator1>: <value1> }, ... }
```

以下查询在 `size` 字段中嵌入的字段 `h` 上使用小于运算符 `$lt`

```javascript
db.inventory.find({
  "size.h": { $lt: 15 }
})
```



##### 指定 AND 条件

以下查询选择嵌套字段 `h` 小于 15，嵌套字段 `uom` 等于 `"in"`，状态字段等于 `"D"` 的所有文档： 

```javascript
db.inventory.find({
  "size.h": { $lt: 15 },
  "size.uom": "in",
  status: "D"
})
```



#### 查询数组

练习之前，先插入测试数据：

```javascript
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```



##### 匹配一个数组

要在数组上指定相等条件，请使用查询文档 `{<field>: <value>}`，其中 `<value>` 是要匹配的精确数组，包括元素的顺序。

下面的示例查询所有文档，其中字段标签值是按指定顺序恰好具有两个元素 `"red"` 和 `"blank"` 的数组：

```javascript
db.inventory.find({
  tags: ["red", "blank"]
})
```



相反，如果您希望找到一个同时包含元素 `"red"` 和 `"blank"` 的数组，而不考虑顺序或该数组中的其他元素，请使用 `$all` 运算符：

```javascript
db.inventory.find({
  tags: { $all: ["red", "blank"] }
})
```



##### 查询数组中的元素

要查询数组字段是否包含至少一个具有指定值的元素，请使用过滤器` {<field>: <value>}`，其中 `<value>` 是元素值。

以下示例查询所有文档，其中 `tag` 是一个包含字符串 `"red"` 作为其元素之一的数组：

```javascript
db.inventory.find({
  tags: "red"
})
```

要在数组字段中的元素上指定条件，请在查询过滤器文档中使用查询运算符：

```javascript
{ <array field>: { <operator1>: <value1>, ... } }
```

例如，以下操作查询数组 `dim_cm` 包含至少一个值大于 25 的元素的所有文档。

```javascript
db.inventory.find({
  dim_cm: { $gt: 25 }
})
```



##### 为数组元素指定多个条件

在数组元素上指定复合条件时，可以指定查询，以使单个数组元素满足这些条件，或者数组元素的任何组合均满足条件。



##### 使用数组元素上的复合过滤条件查询数组

以下示例查询文档，其中 `dim_cm` 数组包含以某种组合满足查询条件的元素；

例如，一个元素可以满足大于 15 的条件，而另一个元素可以满足小于 20 的条件；或者单个元素可以满足以下两个条件：

```javascript
db.inventory.find( { dim_cm: { $gt: 15, $lt: 20 } } )
```



##### 查询满足多个条件的数组元素

使用 [$elemMatch](https://docs.mongodb.com/manual/reference/operator/query/elemMatch/#op._S_elemMatch) 运算符可以在数组的元素上指定多个条件，以使至少一个数组元素满足所有指定的条件。

以下示例查询在 `dim_cm` 数组中包含至少一个同时 大于22  和 小于30 的元素的文档：

```javascript
db.inventory.find({
  dim_cm: { $elemMatch: { $gt: 22, $lt: 30 } }
})
```



##### 通过数组索引位置查询元素

使用点符号，可以为数组的特定索引或位置指定元素的查询条件。该数组使用基于零的索引。

注意：使用点符号查询时，字段和嵌套字段必须在引号内。

下面的示例查询数组 `dim_cm` 中第二个元素大于 25 的所有文档：

```javascript
db.inventory.find( { "dim_cm.1": { $gt: 25 } } )
```



##### 通过数组长度查询数组

使用 `$size` 运算符可按元素数量查询数组。

例如，以下选择数组标签具有3个元素的文档。

```javascript
db.inventory.find( { "tags": { $size: 3 } } )
```



#### 查询嵌入文档的数组

测试数据：

```javascript
db.inventory.insertMany( [
   { item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ] },
   { item: "notebook", instock: [ { warehouse: "C", qty: 5 } ] },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 15 } ] },
   { item: "planner", instock: [ { warehouse: "A", qty: 40 }, { warehouse: "B", qty: 5 } ] },
   { item: "postcard", instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```



##### 查询嵌套在数组中的文档

以下示例选择库存数组中的元素与指定文档匹配的所有文档：

```javascript
db.inventory.find({
  "instock": { warehouse: "A", qty: 5 }
})
```



整个嵌入式/嵌套文档上的相等匹配要求与指定文档（包括字段顺序）完全匹配。例如，以下查询与库存收集中的任何文档都不匹配：

```javascript
db.inventory.find({
  "instock": { qty: 5, warehouse: "A" }
})
```



##### 在文档数组中的字段上指定查询条件

###### 在嵌入文档数组中的字段上指定查询条件

如果您不知道嵌套在数组中的文档的索引位置，请使用点（。）和嵌套文档中的字段名称来连接数组字段的名称。



下面的示例选择所有库存数组中包含至少一个嵌入式文档的嵌入式文档，这些嵌入式文档包含值小于或等于20的字段qty：

```javascript
db.inventory.find( { 'instock.qty': { $lte: 20 } } )
```



###### 使用数组索引在嵌入式文档中查询字段

使用点表示法，您可以为文档中特定索引或数组位置处的字段指定查询条件。该数组使用基于零的索引。

注意：使用点符号查询时，字段和索引必须在引号内。

下面的示例选择所有库存文件，其中库存数组的第一个元素是包含值小于或等于20的字段qty的文档： 

```javascript
db.inventory.find( { 'instock.0.qty': { $lte: 20 } } )
```



##### 为文档数组指定多个条件

在嵌套在文档数组中的多个字段上指定条件时，可以指定查询，以使单个文档满足这些条件，或者数组中文档的任何组合（包括单个文档）都满足条件。



###### 单个嵌套文档在嵌套字段上满足多个查询条件

使用$ elemMatch运算符可在一组嵌入式文档上指定多个条件，以使至少一个嵌入式文档满足所有指定条件。

下面的示例查询库存数组中至少有一个嵌入式文档的文档，这些文档同时包含等于5的字段qty和等于A的字段仓库：

```javascript
db.inventory.find( { "instock": { $elemMatch: { qty: 5, warehouse: "A" } } } )
```



下面的示例查询库存数组中至少有一个嵌入式文档的嵌入式文档包含的字段qty大于10且小于或等于20：

```javascript
db.inventory.find( { "instock": { $elemMatch: { qty: { $gt: 10, $lte: 20 } } } } )
```



###### 元素组合满足标准

如果数组字段上的复合查询条件未使用$ elemMatch运算符，则查询将选择其数组包含满足条件的元素的任意组合的那些文档。

例如，以下查询匹配文档，其中嵌套在库存数组中的任何文档的qty字段都大于10，而数组中的任何文档（但不一定是同一嵌入式文档）的qty字段小于或等于20：

```javascript
db.inventory.find( { "instock.qty": { $gt: 10,  $lte: 20 } } )
```

下面的示例查询库存数组中具有至少一个包含数量等于5的嵌入式文档和至少一个包含等于A的字段仓库的嵌入式文档（但不一定是同一嵌入式文档）的文档：

```javascript
db.inventory.find( { "instock.qty": 5, "instock.warehouse": "A" } )
```



#### 指定从查询返回的项目字段

默认情况下，MongoDB 中的查询返回匹配文档中的所有字段。要限制 MongoDB 发送给应用程序的数据量，可以包含一个投影文档以指定或限制要返回的字段。

测试数据：

```javascript
db.inventory.insertMany( [
  { item: "journal", status: "A", size: { h: 14, w: 21, uom: "cm" }, instock: [ { warehouse: "A", qty: 5 } ] },
  { item: "notebook", status: "A",  size: { h: 8.5, w: 11, uom: "in" }, instock: [ { warehouse: "C", qty: 5 } ] },
  { item: "paper", status: "D", size: { h: 8.5, w: 11, uom: "in" }, instock: [ { warehouse: "A", qty: 60 } ] },
  { item: "planner", status: "D", size: { h: 22.85, w: 30, uom: "cm" }, instock: [ { warehouse: "A", qty: 40 } ] },
  { item: "postcard", status: "A", size: { h: 10, w: 15.25, uom: "cm" }, instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```



##### 返回匹配文档中所有字段

下面的示例返回状态为 `"A"` 的清单集合中所有文档的所有字段：

```javascript
db.inventory.find( { status: "A" } )
```



##### 仅返回指定字段和 `_id` 字段

通过将投影文档中的 `<field>` 设置为 `1`，投影可以显式包含多个字段。以下操作返回与查询匹配的所有文档。在结果集中，在匹配的文档中仅返回项目，状态和默认情况下的 `_id` 字段。

```javascript
db.inventory.find( { status: "A" }, { item: 1, status: 1 } )
```



##### 禁止 `_id` 字段

您可以通过将投影中的 `_id` 字段设置为 `0` 来从结果中删除 `_id` 字段，如以下示例所示：

```javascript
db.inventory.find( { status: "A" }, { item: 1, status: 1, _id: 0 } )
```



##### 返回所有但排除的字段

您可以使用投影排除特定字段，而不用列出要在匹配文档中返回的字段。以下示例返回匹配文档中状态和库存字段以外的所有字段：

```javascript
db.inventory.find( { status: "A" }, { status: 0, instock: 0 } )
```



##### 返回嵌入式文档中的特定字段

您可以返回嵌入式文档中的特定字段。使用点表示法引用嵌入式字段，并在投影文档中将其设置为`1`。

以下示例返回：

- `_id` 字段（默认情况下返回）
- `item` 字段

- `status` 字段
- `size` 文档中的 `uom` 字段

`uom` 字段仍嵌入在尺寸文档中。

```javascript
db.inventory.find(
   { status: "A" },
   { item: 1, status: 1, "size.uom": 1 }
)
```

从MongoDB 4.4 开始，您还可以使用嵌套形式指定嵌入式字段，例如 `{item: 1, status: 1, size: {uom: 1}}`。



##### 禁止嵌入文档中的特定字段

您可以隐藏嵌入式文档中的特定字段。使用点表示法引用投影文档中的嵌入字段并将其设置为`0`。

以下示例指定一个投影，以排除尺寸文档内的 `uom` 字段。其他所有字段均在匹配的文档中返回： 

```javascript
db.inventory.find(
   { status: "A" },
   { "size.uom": 0 }
)
```

从 MongoDB 4.4 开始，您还可以使用嵌套形式指定嵌入式字段，例如 `{ size: { uom: 0 } }`。



##### 在数组中的嵌入式文档上投射

使用点表示法可将特定字段投影在嵌入数组的文档中。

以下示例指定要返回的投影：

- `_id` 字段（默认情况下返回）
- `item` 字段

- `status` 字段
- `qty` 数组中嵌入的文档中的 `instock` 字段

```javascript
db.inventory.find( { status: "A" }, { item: 1, status: 1, "instock.qty": 1 } )
```



##### 返回数组中的项目特定数组元素

对于包含数组的字段，MongoDB 提供以下用于操纵数组的投影运算符：$elemMatch，$slice 和$。

下面的示例使用 $slice 投影运算符返回库存数组中的最后一个元素：

```javascript
db.inventory.find( { status: "A" }, { item: 1, status: 1, instock: { $slice: -1 } } )
```

$elemMatch，$slice 和 $ 是投影要包含在返回数组中的特定元素的唯一方法。例如，您不能使用数组索引来投影特定的数组元素。例如{“ instock.0”：1}投影不会投影第一个元素的数组。



#### 查询空字段或缺少字段

MongoDB 中的不同查询运算符对空值的处理方式不同。

测试数据：

```javascript
db.inventory.insertMany([
   { _id: 1, item: null },
   { _id: 2 }
])
```



##### 相等过滤器

`{item: null}` 查询将匹配包含其值为 `null` 的 `item` 字段或不包含 `item` 字段的文档。

```javascript
db.inventory.find( { item: null } )
```

该查询返回集合中的两个文档。



##### 类型检查

`{ item: { $type: 10 } }` 查询仅匹配包含 `item` 字段，其值为 `null` 的文档；即 `item` 字段的值为 BSON 类型为 Null（类型编号10）：

```javascript
db.inventory.find( { item : { $type: 10 } } )
```

该查询仅返回 `item` 字段值为 `null` 的文档。



##### 存在检查

以下示例查询不包含字段的文档。

`{ { item: { $exists：false }}` 查询与不包含 `item` 字段的文档匹配：

```javascript
db.inventory.find( { item : { $exists: false } } )
```

该查询仅返回不包含项目字段的文档。



### 更新文档

更新操作会修改集合中的现有文档。 MongoDB 提供了以下方法来更新集合的文档：

- `db.collection.updateOne(<filter>, <update>, <options>)`
- `db.collection.updateMany(<filter>, <update>, <options>)`

- `db.collection.replaceOne(<filter>, <update>, <options>)`

您可以指定标识要更新的文档的条件或过滤器。这些过滤器使用与读取操作相同的语法。

![img](MongoDB.assets/1604314104709-3f5831c9-ac49-4c38-aacd-97619bf4ba99.svg)

测试数据：

```javascript
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );
```



#### 语法

为了更新文档，MongoDB 提供了更新操作符（例如 `$set`）来修改字段值。

要使用更新运算符，请将以下形式的更新文档传递给更新方法：

```javascript
{
  <update operator>: { <field1>: <value1>, ... },
  <update operator>: { <field2>: <value2>, ... },
  ...
}
```

如果该字段不存在，则某些更新运算符（例如$ set）将创建该字段。有关详细信息，请参见各个更新操作员参考。



#### 更新单个文档

下面的示例在清单集合上使用 `db.collection.updateOne()` 方法更新项目等于 `paper` 的第一个文档：

```javascript
db.inventory.updateOne(
   { item: "paper" },
   {
     $set: { "size.uom": "cm", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```

更新操作：

- 使用 `$set` 运算符将 `size.uom` 字段的值更新为 `cm`，将状态字段的值更新为 `P`
- 使用 `$currentDate` 运算符将 `lastModified` 字段的值更新为当前日期。如果 `lastModified` 字段不存在，则 `$currentDate` 将创建该字段。



#### 更新多个文档

以下示例在清单集合上使用 `db.collection.updateMany()` 方法来更新数量小于50的所有文档：

```javascript
db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size.uom": "in", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```

更新操作：

- 使用 $set 运算符将 size.uom 字段的值更新为 `"in"`，将状态字段的值更新为 `"p"`
- 使用 `$currentDate` 运算符将 `lastModified` 字段的值更新为当前日期。如果 `lastModified` 字段不存在，则 `$currentDate` 将创建该字段。



#### 替换文档

要替换 _id 字段以外的文档的全部内容，请将一个全新的文档作为第二个参数传递给 `db.collection.replaceOne()`。

替换文档时，替换文档必须仅由字段/值对组成；即不包含更新运算符表达式。

替换文档可以具有与原始文档不同的字段。在替换文档中，由于 `_id` 字段是不可变的，因此可以省略 `_id` 字段；但是，如果您确实包含 `_id` 字段，则它必须与当前值具有相同的值。

以下示例替换了清单集合中项目 `"paper"` 的第一个文档：

```javascript
db.inventory.replaceOne(
   { item: "paper" },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
)
```



### 删除文档

删除操作从集合中删除文档。 MongoDB 提供了以下删除集合文档的方法：

- `db.collection.deleteMany()`
- `db.collection.deleteOne()`

您可以指定标准或过滤器，以标识要删除的文档。这些过滤器使用与读取操作相同的语法。

![img](MongoDB.assets/1604314172728-55b56ab9-9a54-41de-ae9d-5c9b3c724890.svg)



测试数据：

```javascript
db.inventory.insertMany( [
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
] );
```



#### 删除所有文档

要删除集合中的所有文档，请将空的过滤器文档{}传递给db.collection.deleteMany()方法。

以下示例从清单收集中删除所有文档： 

```javascript
db.inventory.deleteMany({})
```

该方法返回具有操作状态的文档。有关更多信息和示例，请参见 deleteMany()。



#### 删除所有符合条件的文档

您可以指定标准或过滤器，以标识要删除的文档。筛选器使用与读取操作相同的语法。

要指定相等条件，请在查询过滤器文档中使用<field>：<value>表达式： 

```javascript
{ <field1>: <value1>, ... }
```



查询过滤器文档可以使用查询运算符以以下形式指定条件：

```javascript
{ <field1>: { <operator1>: <value1> }, ... }
```

要删除所有符合删除条件的文档，请将过滤器参数传递给deleteMany()方法。



以下示例从状态字段等于“ A”的清单集合中删除所有文档：

```javascript
db.inventory.deleteMany({ status : "A" })
```

该方法返回具有操作状态的文档。有关更多信息和示例，请参见deleteMany()。



#### 仅删除1个符合条件的文档

要删除最多一个与指定过滤器匹配的文档（即使多个文档可能与指定过滤器匹配），请使用db.collection.deleteOne()方法。

下面的示例删除状态为“ D”的第一个文档：

```javascript
db.inventory.deleteOne( { status: "D" } )
```



### limit() 方法

类似SQL Limit语句，MongoDB中，使用`limit()`方法限制返回的结果数。

**语法**

`limit()`方法的基本语法如下

```shell
db.COLLECTION_NAME.find().limit(NUMBER)
```

**例子**

假设集合qikegu有以下数据。

```shell
{ "_id" : ObjectId("5cf7b4839ad87fde6fd23a03"), "title" : "MongoDB 介绍" }
{ "_id" : ObjectId("5cf7b5849ad87fde6fd23a05"), "title" : "MongoDB 概述" }
{ "_id" : ObjectId("5cf7b91d9ad87fde6fd23a07"), "title" : "MongoDB 优势" }
```

下面的示例，在查询文档时只显示2个文档。

```shell
> db.qikegu.find({},{"title":1,_id:0}).limit(2)
{ "title" : "MongoDB 介绍" }
{ "title" : "MongoDB 概述" }
```

如果`limit()`方法中没有指定数量参数，将显示集合中的所有文档。



### Skip() 方法

除了`limit()`方法之外，还有一个方法`skip()`也接受number类型参数，用于跳过文档的数量。可以看出，`Skip()`方法的作用类似MySQL Offset语句。

**语法**

`skip()`方法的基本语法如下

```shell
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

**例子**

下面的示例，只显示第二个文档：

```shell
> db.qikegu.find({},{"title":1,_id:0}).limit(1).skip(1)
{ "title" : "MongoDB 概述" }
```

**注意：skip()方法的默认值是0。**





## 在 Node.js 中操作 MongoDB

参考：

- 在服务端操作 MongoDB：https://docs.mongodb.com/drivers/
- 在 Node.js 中操作 MongoDB：https://docs.mongodb.com/drivers/node/



### 初始化示例项目

```shell
mkdir node-mongodb-demo

cd node-mongodb-demo

npm init -y

npm install mongodb
```



### 连接到 MongoDB

```javascript
const { MongoClient } = require("mongodb");
// Connection URI
const uri =
  "mongodb://127.0.0.1:27017";
// Create a new MongoClient
const client = new MongoClient(uri);
async function run() {
  try {
    // Connect the client to the server
    await client.connect();
    // Establish and verify connection
    await client.db("admin").command({ ping: 1 });
    console.log("Connected successfully to server");
  } catch () {
    console.log('Connect failed')
  } finally {
    // Ensures that the client will close when you finish/error
    await client.close();
  }
}

run()
```



### CRUD 操作

CRUD（创建，读取，更新，删除）操作使您可以处理存储在 MongoDB 中的数据。



#### 创建文档

插入一个：

```javascript
const pizzaDocument = {
  name: "Neapolitan pizza",
  shape: "round",
  toppings: [ "San Marzano tomatoes", "mozzarella di bufala cheese" ],
};

const result = await pizzaCollection.insertOne(pizzaDocument);

console.dir(result.insertedCount);
```



插入多个：

```javascript
const pizzaDocuments = [
  { name: "Sicilian pizza", shape: "square" },
  { name: "New York pizza", shape: "round" },
  { name: "Grandma pizza", shape: "square" },
];

const result = pizzaCollection.insertMany(pizzaDocuments);

console.dir(result.insertedCount);
```



#### 查询文档

```javascript
const findResult = await orders.find({
  name: "Lemony Snicket",
  date: {
    $gte: new Date(new Date().setHours(00, 00, 00)),
    $lt: new Date(new Date().setHours(23, 59, 59)),
  },
});
```



#### 删除文档

```javascript
const doc = {
  pageViews: {
    $gt: 10,
    $lt: 32768
  }
};

// 删除符合条件的单个文档
const deleteResult = await collection.deleteOne(doc);
console.dir(deleteResult.deletedCount);

// 删除符合条件的多个文档
const deleteManyResult = await collection.deleteMany(doc);
console.dir(deleteManyResult.deletedCount);
```



#### 修改文档

更新1个文档：

```javascript
const filter = { _id: 465 };
// update the value of the 'z' field to 42
const updateDocument = {
   $set: {
      z: 42,
   },
};

// 更新多个
const result = await collection.updateOne(filter, updateDocument);

// 更新多个
const result = await collection.updateMany(filter, updateDocument);
```



替换文档：

```javascript
const filter = { _id: 465 };
// replace the matched document with the replacement document
const replacementDocument = {
   z: 42,
};
const result = await collection.replaceOne(filter, replacementDocument);
```

**注意：当使用 `_id` 来进行条件查询时，需要先引入`const { ObjectId } = require("mongodb");`，再使用 `ObjectId(xxx)`来包裹 _id 的字符串进行查询。**



## MongoDB 数据库结合 Web 服务

![img](MongoDB.assets/1604889588854-d38893a8-1bdd-4854-bbe1-5dca15fee466.png)

在这次演示中，我们来搭建一个支持 MongoDB 数据库 CRUD 操作的 Web 接口服务，用来进行博客文章的管理。



通过本实战案例，希望你会对数据库及 Web 开发有更深一步的理解。



### 接口设计

基于 RESTful 接口规范。

- [理解 RESTful 架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)
- [RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)



#### 创建文章

- 请求路径：`POST` /articles
- 请求参数：Body

- - title
  - description

- - body
  - tagList

- 数据格式：application/json



请求体示例：

```json
{
  "article": {
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "You have to believe",
    "tagList": ["reactjs", "angularjs", "dragons"]
  }
}
```



返回数据示例：

- 状态码：201
- 响应数据：

```json
{
  "article": {
    "_id": 123,
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "It takes a Jacobian",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z"
  }
}
```



#### 获取文章列表

- 请求路径：`GET` /articles
- 请求参数（Query）

- - _page：页码
  - _size：每页大小



响应数据示例：

- 状态码：200
- 响应数据：

```json
{
  "articles":[{
    "_id": "how-to-train-your-dragon",
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "It takes a Jacobian",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z"
  }, {
    "_id": "how-to-train-your-dragon-2",
    "title": "How to train your dragon 2",
    "description": "So toothless",
    "body": "It a dragon",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z"
  }],
  "articlesCount": 2
}
```



#### 获取单个文章

- 请求路径：`GET` /articles/:id

响应数据示例：

- 状态码：200
- 响应数据：

```json
{
  "article": {
    "_id": "dsa7dsa",
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "It takes a Jacobian",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z"
  }
}
```



#### 更新文章

- 请求路径：`PATCH` /artilces/:id
- 请求参数（Body）

- - title
  - description

- - body
  - tagList

请求体示例：

- 状态码：201
- 响应数据：

```json
{
  "article": {
    "title": "Did you train your dragon?"
  }
}
```

响应示例：

```json
{
  "article": {
    "_id": 123,
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "It takes a Jacobian",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z"
  }
}
```



#### 删除文章

- 接口路径：`DELETE` /articles/:id

响应数据：

- 状态码：204
- 数据：

```json
{}
```



### 准备工作

```shell
mkdir article-bed

cd api-serve

npm init -y

npm i express mongodb
```



#### 使用 Express 快速创建 Web 服务

```javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```



#### 路由设计

```javascript
app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.post('/articles', (req, res) => {
  res.send('post /articles')
})

app.get('/articles', (req, res) => {
  res.send('get /articles')
})

app.get('/articles/:id', (req, res) => {
  res.send('get /articles/:id')
})

app.patch('/articles/:id', (req, res) => {
  res.send('patch /articles/:id')
})

app.delete('/articles/:id', (req, res) => {
  res.send('delete /articles/:id')
})
```



#### 处理 Body 请求数据

```javascript
// 配置解析请求体数据 application/json
// 它会把解析到的请求体数据放到 req.body 中
// 注意：一定要在使用之前就挂载这个中间件
app.use(express.json())
```



### 创建文章

```javascript
app.post('/articles', async (req, res, next) => {
  try {
    // 1. 获取客户端表单数据
    const { article } = req.body

    // 2. 数据验证
    if (!article || !article.title || !article.description || !article.body) {
      return res.status(422).json({
        error: '请求参数不符合规则要求'
      })
    }

    // 3. 把验证通过的数据插入数据库中
    //    成功 -> 发送成功响应
    //    失败 -> 发送失败响应
    await dbClient.connect()

    const collection = dbClient.db('test').collection('articles')

    article.createdAt = new Date()
    article.updatedAt = new Date()
    const ret = await collection.insertOne(article)

    article._id = ret.insertedId
    
    res.status(201).json({
      article
    })
  } catch (err) {
    // 由错误处理中间件统一处理
    next(err)
    // res.status(500).json({
    //   error: err.message
    // })
  }
})
```



### 获取文章列表

```javascript
app.get('/articles', async (req, res, next) => {
  try {
    let { _page = 1, _size = 10 } = req.query
    _page = Number.parseInt(_page)
    _size = Number.parseInt(_size)
    await dbClient.connect()
    const collection = dbClient.db('test').collection('articles')
    const ret = await collection
      .find() // 查询数据
      .skip((_page - 1) * _size) // 跳过多少条 10 1 0 2 10 3 20 n
      .limit(_size) // 拿多少条
    const articles = await ret.toArray()
    const articlesCount = await collection.countDocuments()
    res.status(200).json({
      articles,
      articlesCount
    })
  } catch (err) {
    next(err)
  }
})
```



### 获取单个文章

```javascript
app.get('/articles/:id', async (req, res, next) => {
  try {
    await dbClient.connect()
    const collection = dbClient.db('test').collection('articles')

    const article = await collection.findOne({
      _id: ObjectID(req.params.id)
    })

    res.status(200).json({
      article
    })
  } catch (err) {
    next(err)
  }
})
```



### 更新文章

```javascript
app.patch('/articles/:id', async (req, res, next) => {
  try {
    await dbClient.connect()
    const collection = dbClient.db('test').collection('articles')

    await collection.updateOne({
      _id: ObjectID(req.params.id)
    }, {
      $set: req.body.article
    })

    const article = await await collection.findOne({
      _id: ObjectID(req.params.id)
    })

    res.status(201).json({
      article
    })
  } catch (err) {
    next(err)
  }
})
```



### 删除文章

```javascript
app.delete('/articles/:id', async (req, res, next) => {
  try {
    await dbClient.connect()
    const collection = dbClient.db('test').collection('articles')
    await collection.deleteOne({
      _id: ObjectID(req.params.id)
    })
    res.status(204).json({})
  } catch (err) {
    next(err)
  }
})
```



## mongoose

mongoose 是用来操作 MongoDB 数据库的开源 ORM 框架。它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能。

- 官方网站：https://mongoosejs.com/
- GitHub 仓库：https://github.com/Automattic/mongoose



数据库核心概念区别：

| 分类 | Oralce/Mysql | MongoDB          | Mongoose                 |
| ---- | ------------ | ---------------- | ------------------------ |
| 1    | 数据库实例   | MongoDB实例      | Mongoose                 |
| 2    | 模式(schema) | 数据库(database) | mongoose                 |
| 3    | 表(table)    | 集合(collection) | 模板(Schema)/模型(Model) |
| 4    | 行(row)      | 文档(document)   | 实例(instance)           |
| 5    | Primary key  | Object(_id)      | Object(_id)              |
| 6    | 表自动Column | Field            | Field                    |



**Schema**

参考：https://mongoosejs.com/docs/guide.html。



**SchemaTypes**

参考：https://mongoosejs.com/docs/schematypes.html。



**Connections**

参考：https://mongoosejs.com/docs/connections.html。



**Models**

参考：https://mongoosejs.com/docs/models.html。



**Documents**

参考：https://mongoosejs.com/docs/documents.html。



**Subdocuments**

参考：https://mongoosejs.com/docs/subdocs.html。



**Queries**

参考：https://mongoosejs.com/docs/queries.html。



**Validation**

参考：https://mongoosejs.com/docs/queries.html。



**Populate**

参考：https://mongoosejs.com/docs/populate.html。



Schema：Mongoose 的一切始于 Schema。每个 schema 都会映射到一个 MongoDB collection ，并定义这个collection里的文档的构成。

Model：Models 是从 Schema 编译来的构造函数。 它们的实例就代表着可以从数据库保存和读取的 documents。 从数据库创建和读取 document 的所有操作都是通过 model 进行的。

例如：

```js
var schema = new mongoose.Schema({ name: 'string', size: 'string' });
var Tank = mongoose.model('Tank', schema);
```



### 增删改查

**插入文档：**

```js
const TestSchema = mongoose.Schema({
  name: { type: String },
  age: { type: Number },
  email: { type: String },
})

const User = mongoose.model('users', TestSchema)

const user = {
  name: 'edward',
  age: 28,
  email: '872990547@qq.com'
}

const insertMethod = async () => {
  const data = new User(user)
  const result = await data.save()
  console.log(result)
}
```

**查询文档：**

```js
const findMethods = async () => {
  const result = await User.find()
  console.log(result)
}
```

**修改文档：**

```js
const updateMethods = async () => {
  const result = await User.update()
  console.log(result)
}
```

**删除文档：**

```js
const deleteMethods = async () => {
  const result = await User.deleteOne({name: 'edward'})
  console.log(result)
}
```



### Schemas

#### 实例方法（method）

documents 是 Models 的实例。 Document 有很多自带的实例方法， 当然也可以自定义我们自己的方法。

```js
// define a schema
 var animalSchema = new Schema({ name: String, type: String });

// assign a function to the "methods" object of our animalSchema
animalSchema.methods.findSimilarTypes = function(cb) {
  return this.model('Animal').find({ type: this.type }, cb);
};
```

现在所有 animal 实例都有 findSimilarTypes 方法：

```js
var Animal = mongoose.model('Animal', animalSchema);
var dog = new Animal({ type: 'dog' });

dog.findSimilarTypes(function(err, dogs) {
  console.log(dogs); // woof
});
```

- 重写 mongoose 的默认方法会造成无法预料的结果，相关链接。
- 不要在自定义方法中使用 ES6 箭头函数，会造成 this 指向错误。



#### 静态方法（static）

添加 Model 的静态方法也十分简单，继续用 animalSchema 举例：

```js
// assign a function to the "statics" object of our animalSchema
animalSchema.statics.findByName = function(name, cb) {
  return this.find({ name: new RegExp(name, 'i') }, cb);
};

var Animal = mongoose.model('Animal', animalSchema);
  Animal.findByName('fido', function(err, animals) {
  console.log(animals);
});
```

+ 同样不要在静态方法中使用 ES6 箭头函数。



### Middleware

中间件 (pre 和 post 钩子) 是在异步函数执行时函数传入的控制函数。

#### Pre

##### 串行

串行中间件一个接一个地执行。具体来说， 上一个中间件调用 `next` 函数的时候，下一个执行。

```js
var schema = new Schema(..);
schema.pre('save', function(next) {
  // do stuff
  next();
});
```

使用Pre，可以在保存数据的时候，设置创建时间：

```js
var schema = new Schema(..);
schema.pre('save', function(next) {
  this.created = moment().format('YYYY-MM-DD HH:mm:ss');
  next();
});
```



### Populate

MongoDB是文档型数据库，所以它没有关系型数据库joins特性。但是mongoose也有自己的方法来解决两个表之间的关联问题，Mongoose就是通过populate来解决这个问题的。

[文档参考](https://segmentfault.com/a/1190000021151338)



## Model

**Model.prototype.save()**

新增或更新文档。

- 要 save 的文档不包含 _id 字段，则插入新文档，类似于` insert()`。
- 要 save 的文档包含 _id 字段，则更新文档，相当于 `update(filter,update,{upsert: true})`
- 要 save 的文档包含 id 字段（必须是 ObjectId 形式），但 _id 的值在集合中不存在，则插入新文档；若 _id 的值在集合中存在，id 字段使用文档中的值，不生成新的。



**Model.countDocuments()**

统计与数据库集合中的filter所匹配的文档数。

**参数：**

- filter <Object>
- [callback] <Function>



### 条件操作符

- $eq：等于（=）
- $gt：大于（>）
- $lt：小于（<）
- $gte：大于等于（>=）
- $lte：小于等于（<=）
- $elemMatch：匹配数组字段中的某个值满足 $elemMatch 中指定的所有条件
- $in：匹配数组中指定的任何值
- $regex：正则表达式匹配（等价于mysql中的like模糊匹配）

