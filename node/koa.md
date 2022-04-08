[TOC]

## Koa 基础

### 介绍

- Koa 是一个新的 web 框架，**由 Express 幕后的原班人马打造**，致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。

- - 官网：https://koajs.com/
  - GitHub 仓库：https://github.com/koajs/koa

- - 一个翻译的中文网：https://koa.bootcss.com/

- Koa 的原理和内部结构很像 Express，但是语法和内部结构进行了升级
- Koa **内部使用 ES6 编写**，号称是下一代 Node.js Web 框架

- 它的主要特点是通过**利用 async 函数，帮你丢弃回调函数**

- - Koa 1 是基于 ES2015 中的 Generator 生成器函数结合 CO 模块
  - Koa 2 完全抛弃了 Generator 和 co，升级为了 ES2017 中的 async/await 函数

- 正式由于 Koa 内部基于最新的异步处理方式，所以使用 **Koa 处理异常更加简单**
- Koa 中提供了 CTX 上下文对象

- - Express 是扩展了 req 和 res

- **Koa 并没有捆绑任何中间件**， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。
- 有很多开发工具/框架都是基于 Koa 的

- - [Egg.js](https://eggjs.org/)
  - 构建工具 [Vite](https://github.com/vitejs/vite)

- [Koa vs Express](https://github.com/koajs/koa/blob/master/docs/koa-vs-express.md)
- 个人评价

- - koa 2 好用，设计上的确有优势。优势不在能实现更强的功能，而是可以更简单地完成功能。
  - koa 2 社区远不如 express

- - koa 1 在思想上与 koa 2 是一致的，但是 koa 2 的实现更漂亮

- [Awesome Koa](https://github.com/ellerbrock/awesome-koa)



### Koa 基本用法

1、安装 koa

```shell
npm i koa
```

Koa 依赖 node v7.6.0 或 ES2015及更高版本和 async 方法支持。



2、app.js

```javascript
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

- Koa 应用程序是一个包含一组中间件函数的对象
- 它是按照类似堆栈的方式组织和执行的

- Koa 内部没有捆绑任何中间件，甚至是路由功能



### Koa 中的 Context 对象

参见：https://koa.bootcss.com/#context。



### Koa 中的路由

#### 原生路由

网站一般都有多个页面。通过ctx.request.path可以获取用户请求的路径，由此实现简单的路由。

```javascript
const main = ctx => {
  if (ctx.request.path !== '/') {
    ctx.response.type = 'html';
    ctx.response.body = '<a href="/">Index Page</a>';
  } else {
    ctx.response.body = 'Hello World';
  }
};
```



#### koa-router 模块

原生路由用起来不太方便，我们可以使用封装好的 [koa-router](https://github.com/koajs/router) 模块。



- Express 路由风格（app.get、app.put、app.post ...）
- 命名动态 URL 参数

- 具有 URL 生成的命名路由
- 使用允许的请求方法响应 OPTIONS 请求

- 支持 405 和 501 响应处理
- 支持多路由中间件

- 支持多个嵌套的路由中间件
- 支持 async/awai 语法



1、安装

```shell
npm i @koa/router
```

2、示例

```javascript
const Koa = require('koa');
const Router = require('@koa/router');

const app = new Koa();
const router = new Router();

router.get('/', (ctx, next) => {
  // ctx.router available
});

app
  .use(router.routes())
  .use(router.allowedMethods());
```





### 静态资源托管

如果网站提供静态资源（图片、字体、样式表、脚本......），为它们一个个写路由就很麻烦，也没必要。[koa-static](https://github.com/koajs/static) 模块封装了这部分的请求。



1、安装

```shell
npm install koa-static
```

2、示例

```javascript
const serve = require('koa-static');
const Koa = require('koa');
const app = new Koa();

// $ GET /package.json
app.use(serve('.'));

// $ GET /hello.txt
app.use(serve('test/fixtures'));

// or use absolute paths
app.use(serve(__dirname + '/test/fixtures'));

app.listen(3000);

console.log('listening on port 3000');
```

#### 给静态资源设置虚拟路径

使用 Koa 提供的 [koa-mount](https://github.com/koajs/mount) 。



### 重定向

有些场合，服务器需要重定向（redirect）访问请求。比如，用户登陆以后，将他重定向到登陆前的页面。ctx.response.redirect()方法可以发出一个302跳转，将用户导向另一个路由。

```javascript
ctx.response.redirect('/');
ctx.response.body = '<a href="/">Index Page</a>';
```



### Koa 中间件

#### Logger 功能

Koa 的最大特色，也是最重要的一个设计，就是中间件（middleware）。为了理解中间件，我们先看一下 Logger （打印日志）功能的实现。

```javascript
const main = ctx => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  ctx.response.body = 'Hello World';
};
```



#### 中间件栈

![img](koa.assets/1608778328734-8e6bba65-d000-4694-84f7-aeb85c80ea8b.png)

- 多个中间件会形成一个栈结构（middle stack），以"先进后出"（first-in-last-out）的顺序执行。
- 最外层的中间件首先执行。

- 调用next函数，把执行权交给下一个中间件。
- ...

- 最内层的中间件最后执行。
- 执行结束后，把执行权交回上一层的中间件。

- ...
- 最外层的中间件收回执行权之后，执行next函数后面的代码。



中间件栈结构示例如下：

```javascript
const one = (ctx, next) => {
  console.log('>> one');
  next();
  console.log('<< one');
}

const two = (ctx, next) => {
  console.log('>> two');
  next(); 
  console.log('<< two');
}

const three = (ctx, next) => {
  console.log('>> three');
  next();
  console.log('<< three');
}

app.use(one);
app.use(two);
app.use(three);
```

如果中间件内部没有调用 `next` 函数，那么执行权就不会传递下去。作为练习，你可以将 `two` 函数里面 `next()` 这一行注释掉再执行，看看会有什么结果。



#### 异步中间件

迄今为止，所有例子的中间件都是同步的，不包含异步操作。如果有异步操作（比如读取数据库），中间件就必须写成 async 函数。

```javascript
app.use(async (ctx, next) => {
  const data = await util.promisify(fs.readFile)('./views/index.html')
  ctx.type = 'html'
  ctx.body = data
  next()
})
```

上面代码中，`fs.readFile` 是一个异步操作，必须写成 await `fs.readFile()`，然后中间件必须写成 async 函数。



#### 中间件的合成

[koa-compose](https://github.com/koajs/compose) 模块可以将多个中间件合成为一个。



1、安装

```shell
npm install koa-compose
```

2、示例

```javascript
const compose = require('koa-compose');

const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}

const main = ctx => {
  ctx.response.body = 'Hello World';
};

const middlewares = compose([logger, main]);
app.use(middlewares);
```



### Koa 中的错误处理

#### 500 错误

如果代码运行过程中发生错误，我们需要把错误信息返回给用户。HTTP 协定约定这时要返回500状态码。Koa 提供了ctx.throw()方法，用来抛出错误，ctx.throw(500)就是抛出500错误。

```javascript
const main = ctx => {
  ctx.throw(500);
};
```



#### 404 错误

如果将ctx.response.status设置成404，就相当于ctx.throw(404)，返回404错误。

```javascript
const main = ctx => {
  ctx.response.status = 404;
  ctx.response.body = 'Page Not Found';
};
```



#### 处理错误的中间件

为了方便处理错误，最好使用 `try...catch` 将其捕获。但是，为每个中间件都写 `try...catch` 太麻烦，我们可以让最外层的中间件，负责所有中间件的错误处理。

```javascript
const handler = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500;
    ctx.response.body = {
      message: err.message
    };
  }
};

const main = ctx => {
  ctx.throw(500);
};

app.use(handler);
app.use(main);
```



#### error 事件的监听

运行过程中一旦出错，Koa 会触发一个error事件。监听这个事件，也可以处理错误。

```javascript
const main = ctx => {
  ctx.throw(500);
};

app.on('error', (err, ctx) =>
  console.error('server error', err);
);
```

如果 req/res 期间出现错误，并且 _无法_ 响应客户端，Context实例仍然被传递：

```javascript
app.on('error', (err, ctx) => {
  log.error('server error', err, ctx)
});
```

当发生错误并且仍然可以响应客户端时，也没有数据被写入 socket 中，Koa 将用一个 500 “内部服务器错误” 进行适当的响应。在任一情况下，为了记录目的，都会发出应用级 “错误”。



#### 释放 error 事件

需要注意的是，如果错误被try...catch捕获，就不会触发error事件。这时，必须调用ctx.app.emit()，手动释放error事件，才能让监听函数生效。

```javascript
const handler = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500;
    ctx.response.type = 'html';
    ctx.response.body = '<p>Something wrong, please contact administrator.</p>';
    ctx.app.emit('error', err, ctx);
  }
};

const main = ctx => {
  ctx.throw(500);
};

app.on('error', function(err) {
  console.log('logging error ', err.message);
  console.log(err);
});
```

上面代码中，`main` 函数抛出错误，被 `handler` 函数捕获。`catch` 代码块里面使用 `ctx.app.emit()` 手动释放 `error` 事件，才能让监听函数监听到。





## 常用中间件



### 路由 koa-router



### 协议解析 koa-body



### 跨域处理 @koa/cors



### json格式美化 koa-json



### 路由压缩 koa-combine-routers



### 安全header处理 koa-helmet



### 静态资源 koa-static



### 打印请求响应日志 koa-logger



### 集成中间件 koa-compose



### 压缩中间件 koa-compress





## Koa 原理实现

### Koa 源码目录结构

```shell
.
├── History.md
├── LICENSE
├── Readme.md
├── dist
│   └── koa.mjs
├── lib
│   ├── application.js  # 最核心的模块
│   ├── context.js # 上下文对象
│   ├── request.js # Koa 自己实现的请求对象
│   └── response.js # Koa 自己实现的响应对象
└── package.json
```



### 基本结构

使用：

```javascript
const Koa = require('./koa')

const app = new Koa()

app.listen(3000)
```

`application.js`

```javascript
module.exports = class Application {
  listen(...args) {
    const server = http.createServer((req, res) => {
      res.end('My Koa')
    })
    return server.listen(...args)
  }
}
```



### 实现中间件功能

- Koa 会把所有中间件组合成一个大的 Promise
- 当这个 Promise 执行完毕之后，会采用当前的 ctx.body 进行结果响应

- next 前面必须有 await 或者 return next，否则执行顺序达不到预期
- 如果都是同步执行，加不加 await 都无所谓

- 我不知道后续是否有异步逻辑，所以建议写的时候都加上 await next



#### 收集中间件

使用方式：

```javascript
const Koa = require('./koa')

const app = new Koa()

app.use((ctx, next) => {
  ctx.body = 'foo'
})

app.use((ctx, next) => {
  ctx.body = 'Koa'
})

app.listen(3000)
```

`application.js`

```javascript
const http = require('http')

module.exports = class Application {
  constructor () {
    this.middleware = []
  }

  use(fn) {
    if (typeof fn !== 'function')
      throw new TypeError('middleware must be a function!')
    this.middleware.push(fn)
  }

  listen(...args) {
    const server = http.createServer((req, res) => {
      res.end('My Koa')
    })
    return server.listen(...args)
  }
}
```



#### 调用中间件

```javascript
const http = require('http')

module.exports = class Application {
  constructor() {
    this.middleware = [] // 存储用户所有的中间件处理函数
  }

  use(fn) {
    this.middleware.push(fn)
  }

  listen(...args) {
    const server = http.createServer(this.callback())
    return server.listen(...args)
  }

  compose(middleware) {
    return function () { // 可以配置额外参数
      const dispatch = i => {
        if (i >= middleware.length) return Promise.resolve()
        const fn = middleware[i]
        return Promise.resolve(
          fn({}, () => dispatch(i + 1))
        )
      }
      return dispatch(0)
    }
  }

  callback() {
    const fnMiddleware = this.compose(this.middleware)
    const handleRequest = (req, res) => {
      fnMiddleware()
        .then(() => {
          // 处理结束
          res.end('end')
        })
        .catch(err => {
          // 发生异常
          res.end('error')
        })
    }
    return handleRequest
  }
}
```



### 处理上下文对象

#### 初始化上下文对象

context 上下文对象的使用方式：

```javascript
/**
 * Koa Context
 */


const Koa = require('./koa')

const app = new Koa()

// Koa Context 将 node 的 request 和 response 对象封装到单个对象中，为编写 Web 应用程序和 API 提供了许多有用的方法。
// 每个请求都将创建一个 Context，并在中间件中作为参数引用
app.use((ctx, next) => {
  console.log(ctx) // Context 对象

  console.log(ctx.req) // Node 的 request 对象
  console.log(ctx.res) // Node 的 response 对象
  
  console.log(ctx.request) // Koa 中封装的请求对象
  console.log(ctx.request.header) // 获取请求头对象
  console.log(ctx.request.method) // 获取请求方法
  console.log(ctx.request.url) // 获取请求路径
  console.log(ctx.request.path) // 获取不包含查询字符串的请求路径
  console.log(ctx.request.query) // 获取请求路径中的查询字符串

  // Request 别名
  // 完整列表参见：https://koa.bootcss.com/#request-
  console.log(ctx.header)
  console.log(ctx.method)
  console.log(ctx.url)
  console.log(ctx.path)
  console.log(ctx.query)


  // Koa 中封装的响应对象
  console.log(ctx.response)
  
  // Response 别名
  ctx.status = 200
  ctx.message = 'Success'
  ctx.type = 'plain'
  ctx.body = 'Hello Koa'
  
})

app.listen(3000)
```



`context.js`

```javascript
const context = {
  
}

module.exports = context
```

`request.js`

```javascript
const url = require('url')

const request = {
  get url () {
    return this.req.url
  },

  get path () {
    return url.parse(this.req.url).pathname
  },

  get query () {
    return url.parse(this.req.url, true).query
  },

  get method () {
    return this.req.method
  }
}

module.exports = request
```

`response.js`

```javascript
const response = {}

module.exports = response
```

`application.js`

```javascript
const http = require('http')
const context = require('./context')
const request = require('./request')
const response = require('./response')

class Application {
  constructor() {
    this.middleware = [] // 保存用户添加的中间件函数

    // 为了防止多个实例共享 Request、Response、Context 导致数据被意外修改，这里重新拷贝一份
    this.request = Object.create(request)
    this.response = Object.create(response)
    this.context = Object.create(context)
  }

  listen(...args) {
    const server = http.createServer(this.callback())
    server.listen(...args)
  }

  use(fn) {
    this.middleware.push(fn)
  }

  // 异步递归遍历调用中间件处理函数
  compose(middleware) {
    return function (context) {
      const dispatch = index => {
        if (index >= middleware.length) return Promise.resolve()
        const fn = middleware[index]
        return Promise.resolve(
          // TODO: 上下文对象
          fn(context, () => dispatch(index + 1)) // 这是 next 函数
        )
      }

      // 返回第 1 个中间件处理函数
      return dispatch(0)
    }
  }

  createContext(req, res) {
    // 为了避免请求之间 context 数据交叉污染，这里为每个请求单独创建 context 请求对象
    const context = Object.create(this.context)
    const request = Object.create(this.request)
    const response = Object.create(this.response)

    context.app = request.app = response.app = this
    context.req = request.req = response.req = req // 原生的请求对象
    context.res = request.res = response.res = res // 原生的响应对象
    request.ctx = response.ctx = context // 在 Request 和 Response 中也可以拿到 context 上下文对象
    request.response = response // Request 中也可以拿到 Response
    response.request = request // Response 中也可以拿到 Request
    context.originalUrl = request.originalUrl = req.url // 没有经过任何处理的请求路径
    context.state = {} // 初始化 state 数据对象，用于给模板视图提供数据

    return context
  }

  callback() {
    const fnMiddleware = this.compose(this.middleware)
    const handleRequest = (req, res) => {
      // 1、创建上下文请求对象
      const context = this.createContext(req, res)

      // 2、传入上下文对象执行中间件栈
      fnMiddleware(context)
        .then(() => {
          console.log('end')
          res.end('My Koa')
        })
        .catch(err => {
          res.end(err.message)
        })
    }

    return handleRequest
  }
}

module.exports = Application
```



#### 处理上下文对象中的别名

`context.js`

```javascript
const context = {
  get method() {},
};

defineProperty('request', 'method')
defineProperty('request', 'url')
defineProperty('response', 'body')

function defineProperty(target, name) {
  // 定义对象属性描述符的 getter 和 setter
  Object.defineProperty(context, name, {
    get() {
      return this[target][name];
    },

    set(value) {
      this[target][name] = value
    }
  });
}

module.exports = context;
```



### 处理 ctx.body

#### 保存 body 数据

`response.js`

```javascript
const response = {
  set status(value) {
    this.res.statusCode = value;
  },

  _body: "", // 真正用来存数据的

  get body() {
    return this._body;
  },

  set body(value) {
    this._body = value;
  },
};

module.exports = response;
```



#### 发送 body 数据

`application.js`

```javascript
class Application {
  callback() {
  	const fnMiddleware = this.compose(this.middleware);
  	const handleRequest = (req, res) => {
      // 每个请求都将创建一个独立的 Context 上下文对象，它们直接不会互相污染
      const context = this.createContext(req, res);
      fnMiddleware(context)
        .then(() => {
        	res.end(context.body); // 发送 body 数据
        })
        .catch((err) => {
          console.log(err);
        });
    };
    return handleRequest;
  }
}
```



#### 更灵活的 body 数据

`application.js`

```javascript
class Application {
  callback() {
  	const fnMiddleware = this.compose(this.middleware);
  	const handleRequest = (req, res) => {
      // 每个请求都将创建一个独立的 Context 上下文对象，它们直接不会互相污染
      const context = this.createContext(req, res);
      fnMiddleware(context)
        .then(() => {
          respond(context);
          // res.end(context.body); // 发送 body 数据
        })
        .catch((err) => {
          console.log(err);
        });
    };
    return handleRequest;
  }
}

function respond(ctx) {
  const { body, res } = ctx;
  if (body === null) {
    res.statusCode = 204; // 204 Not Content 没有内容返回
    return res.end();
  }

  if (typeof body === "string") return res.end(body);
  if (Buffer.isBuffer(body)) return res.end(body);
  if (body instanceof Stream) return body.pipe(res);
  if (typeof body === "number") return res.end(body + "");
  if (typeof body === "object") {
    const jsonStr = JSON.stringify(body);
    return res.end(jsonStr);
  }
}
```

