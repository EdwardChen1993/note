[TOC]

# Mongodb

## 安装

安装包方式：[官方文档](https://docs.mongodb.com/manual/administration/install-community/)

docker方式：[官方文档](https://hub.docker.com/_/mongo)

安装成功后，使用命令验证是否安装成功：

```bash
mongo -version
```

![image-20210427144814804](Mongodb.assets/image-20210427144814804.png)

然后查看目录，先启动mongod，再启动mongo：

![image-20210427144701290](Mongodb.assets/image-20210427144701290.png)

连接成功，Mongodb默认端口是27017：

![image-20210427164403409](Mongodb.assets/image-20210427164403409.png)

mongod 命令是启用数据库服务，即搭建并开启服务器，可以通过端口被访问（27017）

mongo 命令是连接数据库服务，即连接服务器，可以通过端口进行访问（27017）

注意：mongod 和 mongo 的区别，前者是启用Mongodb进程，后者是对Mongodb进行连接操作。



## 备份/恢复

**备份：**

```bash
mongodump -h localhost -u root -p 密码 -d test -o /tmp/test
```

参数：

- -h：Mongodb 所在服务器地址
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

- -h：Mongodb 所在服务器地址
- -u：用户名
- -p：密码
- -d：需要备份的数据库，例如：test，不传-d则备份所有数据库。
- --dir：指定备份存放的目录。



## mongoose

[官方文档](http://www.mongoosejs.net/)

Mongoose是一个对象文档模型（ODM）库，它对Node原生的Mongodb模块进行了进一步的优化封装，并提供了更多的功能。

数据库核心概念区别：

| 分类 | Oralce/Mysql | Mongodb          | Mongoose                 |
| ---- | ------------ | ---------------- | ------------------------ |
| 1    | 数据库实例   | Mongodb实例      | Mongoose                 |
| 2    | 模式(schema) | 数据库(database) | mongoose                 |
| 3    | 表(table)    | 集合(collection) | 模板(Schema)/模型(Model) |
| 4    | 行(row)      | 文档(document)   | 实例(instance)           |
| 5    | Primary key  | Object(_id)      | Object(_id)              |
| 6    | 表自动Column | Field            | Field                    |

Schema：Mongoose 的一切始于 Schema。每个 schema 都会映射到一个 Mongodb collection ，并定义这个collection里的文档的构成。

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

Mongodb是文档型数据库，所以它没有关系型数据库joins特性。但是mongoose也有自己的方法来解决两个表之间的关联问题，Mongoose就是通过populate来解决这个问题的。

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