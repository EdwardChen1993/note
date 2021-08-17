[TOC]

# Nuxt

Nuxt 是一个基于 Vue 的通用应用框架，预设了利用 Vue 开发服务端渲染所需要的各种配置。提供静态站点、异步数据加载、中间件支持、布局支持等。



## 文档

[官方文档](https://zh.nuxtjs.org/)



## 区别

Vue 和 Nuxt 的区别：

| 分类     | Vue                      | Nuxt                                         |
| -------- | ------------------------ | -------------------------------------------- |
| 框架     | 独立框架                 | 基于Vue，不仅用于服务端渲染，还进行了丰富    |
| 生命周期 | 全                       | 只有beforeCreated，created                   |
| 组件     | router-view，router-link | nuxt，nuxt-child，nuxt-link，**client-only** |
| 路由     | 自定义                   | 由文件名、文件夹**自动生成**                 |
| 目录结构 | 自定义                   | 相对限定，不同的文件名不同的**默认行为**     |
| 第三方库 | 自定义                   | 需求分浏览器与node侧                         |
| 其他     | cli集成了vuex，router    | prettier，ui框架等                           |



## 工程目录

```
.
├── layouts       // 视图
├── pages         // 页面、用于形成路由
├── components    // 存放业务组件
├── assets    		// 预编译资源 sass
├── plugins    		// 插件配置
├── middlewares   // 模块
├── static        // 静态资源
└── store  				// vuex
. 
```



## 异步数据

`asyncData` 方法会在组件（限于页面组件）每次加载之前被调用。它可以在服务端或路由更新之前被调用。在这个方法被调用的时候，第一个参数被设定为当前页面的上下文对象，你可以利用 `asyncData`方法来获取数据，Nuxt.js 会将 `asyncData` 返回的数据融合组件 `data `方法返回的数据一并返回给当前组件。

示例：

```vue
<template>
  <div>
    <h1>{{ post.title }}</h1>
    <p>{{ post.description }}</p>
  </div>
</template>

<script>
  export default {
    async asyncData({ params, $http }) {
      const post = await $http.$get(`https://api.nuxtjs.dev/posts/${params.id}`)
      return { post }
    }
  }
</script>
```

