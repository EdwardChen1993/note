[TOC]

# koa

koa 是由 Express 原班人马打造的，致力于成为一个更小、更富有表现力、更健壮的 Web 框架。使用 koa 编写 web 应用，通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套，并极大地提升错误处理的效率。koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。



## 洋葱模型

![clipboard.png](https://segmentfault.com/img/bV6DZG?w=478&h=435)

![clipboard.png](https://segmentfault.com/img/bV6D5Z?w=470&h=411)

示例代码：

```js
const Koa = require('koa');

// 应用程序
const app = new Koa();

// 中间件1
app.use((ctx, next) => {
    console.log(1);
    next();
    console.log(2);
});

// 中间件2
app.use((ctx, next) => {
    console.log(3);
    next();
    console.log(4);
});

app.listen(9000, '0.0.0.0', () => {
    console.log(`Server is starting`);
});

打印输出
// 1
// 3
// 4
// 2
```

在koa的中间件中，通过next函数，将中间件分成了两部分，next上面的一部分会首先执行，而下面的一部分则会在所有后续的中间件调用之后执行。其实不难发现，会有两次进入同一个中间件的行为，而且是在所有第一次的中间件执行之后，才依次返回上一个中间件。所有的请求进过一个中间件都会被执行两次，这样可以很方便的做一些后置处理。



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



